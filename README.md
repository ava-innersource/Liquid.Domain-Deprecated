Liquid Application Framework - Domain
=====================================

This repository is part of the [Liquid Application Framework](https://github.com/Avanade/Liquid-Application-Framework), a modern Dotnet Core Application Framework for building cloud native microservices.

The main repository contains the examples and documentation on how to use Liquid.

Liquid Domain
-------------
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Domain&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Domain)


This package contains the subsystem of Liquid responsible for abstracting behavior and injection of domain handlers.

Getting Started
===
To use this package, your command request must implement ```IRequest``` from MediatR, and your command handler must implement ```IRequestHandler``` .

```C#
using MediatR;
```

```C#
public class SampleRequest : IRequest<SampleResponse>
{
    //declare the parameters or body of the request here, if its needed.
}
```
```C#
public class SampleQueryHandler : IRequestHandler<SampleRequest, SampleResponse>
    {  

        public async Task<SampleResponse> Handle(SampleRequest request, CancellationToken cancellationToken)
        {
             return new SampleResponse("Hello World!");
        }
    }
```

If you want to validate the input before processing, it is as easy as implementing a class that inherits ```AbstractValidator``` from FluentValidation.

```C#
using FluentValidation;
```
```C#
 public class MyCommandValidator : AbstractValidator<MyCommand>
{
    public MyCommandValidator()
    {
        RuleFor(command => command.Name).NotEmpty();
    }
}
```
This package also provides a dependency injection extension method to register your domain handlers and Liquid Mediator Behaviors for validation and telemetry:

```C#
using Liquid.Domain.Extensions.DependencyInjection;
```
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLiquidHandlers(withTelemetry: true, withValidators: true, typeof(MyCommand).Assembly);
}
```