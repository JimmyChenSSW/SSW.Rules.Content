---
seoDescription: Learn when to implement IDisposable and ensure efficient memory management in .NET by understanding how to control unmanaged resources with this guide.
type: rule
title: Do you know when to implement IDisposable?
uri: when-to-implement-idisposable
authors: []
related: []
redirects:
  - do-you-know-when-to-implement-idisposable
created: 2019-01-11T23:59:37.000Z
archivedreason: null
guid: 2543135e-771c-4536-a79d-391561e272ec
---

If you access unmanaged resources (e.g. files, database connections etc.) in a class, you should implement `IDisposable` and overwrite the `Dispose` method to allow you to control when the memory is freed.  If not, this responsibility is left to the garbage collector to free the memory when the object containing the unmanaged resources is finalized. This means the memory will be unnecessarily consumed by resources which are no longer required, which can lead to inefficient performance and potentially running out of memory altogether.

<!--endintro-->

```cs
public class MyClass
{
   private File myFile = File.Open(...); // This is an unmanaged resource
}

//elsewhere in project:
private void useMyClass()
{
  var myClass = new MyClass();
   /*
  Here we are using an unmanaged resource without disposing of it, meaning it will hang around in memory unnecessarily until the garbage collector finalizes it
  */
 }
```

::: bad
Figure: Bad example - Using unmanaged resources without disposing of them when we are done

:::

```cs
public class MyClass : IDisposable
{
  private File myFile = new File.Open(...); // This is an unmanaged resource

  public void Dispose()
  {
    myFile.Dispose(); // Here we dispose of the unmanaged resource
    GC.SuppressFinalize(this); // Preventing a redundant garbage collector finalize call
  }
}
```

::: good
Figure: Good example - Implementing `IDisposable` allows you to dispose of the unmanaged resources deterministically to maximize efficiency

:::

Now we can use the `using` statement to automatically dispose the class when you are finished with it.:

```cs
private void useClass()
{
  using (var myClass = new MyClass())
  {
    // do stuff with myClass here...
  }  // myClass.Dispose() is automatically run at the end of the using block
}
```

::: good
Figure: Good example - With the `using` statement, the unmanaged resources are disposed of as soon as we are finished with them

:::

[See here](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose.aspx) for more details.
