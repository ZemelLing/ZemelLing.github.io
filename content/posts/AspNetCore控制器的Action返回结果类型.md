---
title: "AspNetCore控制器的Action返回结果类型"
date: 2021-07-02T12:48:31+08:00
draft: false
---

# 特定类型(Specific type)

Action可返回原始类型或复杂类型对象（例如 string 或 自定义类型）。以下代码显示返回自定义的Product类型对象：

```C#
[HttpGet]
public List<Product> Get() =>
    _repository.GetProducts();
```

当同时返回多个对象时，需要返回 ActionResult 类型的对象，将其与前边的多个对象整合。

## 返回 IEnumerable<T> 或 IAsyncEnumerable<T> 类型

在 3.0 及之后的版本中返回 IAsyncEnumerable<T> 类型结果
1. 不会导致Action阻塞
2. 与返回 IEnumerabel<T> 一样

在 3.0 及之后的版本返回 IEnumerable<T> 结果在序列化之前会被缓存：

```c#
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```
以下两段代码在 3.0 及之后的版本中都不会造成Action阻塞:

```c#
[HttpGet("syncsale")]
public IEnumerable<Product> GetOnSaleProducts()
{
    var products = _repository.GetProducts();

    foreach (var product in products)
    {
        if (product.IsOnSale)
        {
            yield return product;
        }
    }
}
```

```c#
[HttpGet("asyncsale")]
public async IAsyncEnumerable<Product> GetOnSaleProductsAsync()
{
    var products = _repository.GetProductsAsync();

    await foreach (var product in products)
    {
        if (product.IsOnSale)
        {
            yield return product;
        }
    }
}
```

# IActionResult 类型

当需要返回多个不同的 ActionResult 时，应该设置Action的返回类型为 IActionResult。ActionResult 类型表示各种 HTTP 状态码。任何基础自 ActionResult 的非抽象类型都是合法的返回类型。常见的ActionResult的实现类型有 BadRequestResult (400), NotFoundResult (404), 及 OkObjectResult (200) 等。或者使用 ControllerBase 类型中的便利方法返回这些类型，例如，BadRequest()方法返回 new BadRequestResult()。

当需要返回多个不同的 ActionResult 时，不仅需要设置 Action 的返回类型为 IActionResult，而且需要为 Action 设置 [ProducesResponseType] 特性，用于指定可能的返回类型。该特性将为 WebApi 文档（Swagger）增加关于返回类型的描述信息（HTTP 状态码等）

## 同步Action

```c#
[HttpGet("{id}")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
public IActionResult GetById(int id)
{
    if (!_repository.TryGetProduct(id, out var product))
    {
        return NotFound();
    }

    return Ok(product);
}
```

## 异步Action

```c#
[HttpPost]
[Consumes(MediaTypeNames.Application.Json)]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public async Task<IActionResult> CreateAsync(Product product)
{
    if (product.Description.Contains("XYZ Widget"))
    {
        return BadRequest();
    }

    await _repository.AddProductAsync(product);

    return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
}
```

* CreatedAtAction 方法返回 CreatedAtActionResult 对象，其在返回 201 状态码的同时在Header中附加了Location用于返回新建的Product的URL

以下代码指定了 Product 中必须包含 Name 和 Description 属性，若在Action参数中不包含其中的其中任意一个，将导致model验证失败：

```c#
public class Product
{
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [Required]
    public string Description { get; set; }
}
```

# ActionResult<T> 类型

指定了 ActionResult<T> 作为返回类型的 Action 可返回继承自 ActionResult 的类型或特定类型。ActionResult<T> 与 IActionResult 相比具有以下优点：

* 可以不必指定 [ProducesResponseType] 特性的 Type 属性，例如：[ProducesResponseType(200, Type = typeof(Product))] 可简化为 [ProducesResponseType(200)]。该 Action 的预期返回类型可从 ActionResult<T> 中推断出。

* 可将返回的 T 或 ActionResult 隐式转换为 ActionResult<T>

c#不支持接口类型的隐式转换，因此需要使用 ActionResult<T> 将接口转换为具体类型，例如，以下使用 IEnumerable 的代码将不起作用：

```c#
// 这是有误的代码
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

以上代码将 return 的语句改为 `_repository.GetProducts().ToList();` 就无误了。

## 同步Action

```c#
[HttpGet("{id}")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
public ActionResult<Product> GetById(int id)
{
    if (!_repository.TryGetProduct(id, out var product))
    {
        return NotFound();
    }

    return product;
}
```

以上代码当Id存在时将返回200状态码以及Product数据

## 异步Action


```c#
[HttpPost]
[Consumes(MediaTypeNames.Application.Json)]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public async Task<ActionResult<Product>> CreateAsync(Product product)
{
    if (product.Description.Contains("XYZ Widget"))
    {
        return BadRequest();
    }

    await _repository.AddProductAsync(product);

    return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
}
```