---
title: Model Validation
description: Model Validation .Net
sidebar_label: "Introduction"
sidebar_position: 1
---

When a client sends data to your web API, often you want to validate the data before doing any processing.

## Validation Attributes

Validation attributes let you specify validation rules for model properties.

```csharp
public record CreateBreakfastRequest(
    [MinLength(3),MaxLength(10)]
    string Name,
    string Description,
    [Future]
    DateTime StartDateTime,
    [Future]
    DateTime EndDateTime,
    List<string> Savory,
    List<string> Sweet);
```

### Built-in attributes

- [ValidateNever]: Indicates that a property or parameter should be excluded from validation.
- [CreditCard]: Validates that the property has a credit card format. Requires jQuery Validation Additional Methods.
- [Compare]: Validates that two properties in a model match.
- [EmailAddress]: Validates that the property has an email format.
- [Phone]: Validates that the property has a telephone number format.
- [Range]: Validates that the property value falls within a specified range.
- [RegularExpression]: Validates that the property value matches a specified regular expression.
- [Required]: Validates that the field isn't null. See [Required] attribute for details about this attribute's behavior.
- [StringLength]: Validates that a string property value doesn't exceed a specified length limit.
- [Url]: Validates that the property has a URL format.
- [Remote]: Validates input on the client by calling an action method on the server. See [Remote] attribute for details about this attribute's behavior.

### Error messages

Validation attributes let you specify the error message to be displayed for invalid input. For example:

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

### Custom Validation

For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes. Create a class that inherits from `ValidationAttribute`, and override the `IsValid` method.

The IsValid method accepts an object named value, which is the input to be validated. An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.

```csharp
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter,
AllowMultiple = false)]
public class FutureAttribute : ValidationAttribute
{

    protected override ValidationResult? IsValid(object? value,
    ValidationContext validationContext)
    {
        return value is DateTime dateTime && dateTime > DateTime.UtcNow
        ? ValidationResult.Success
        : new ValidationResult("Date must be in the future");
    }

}
```

## Fluent Validation

Using the NuGet package manager console within Visual Studio run the following command:

```csharp
Install-Package FluentValidation
```

Or using the .net core CLI from a terminal window:

```csharp
dotnet add package FluentValidation
```

To implement a validator with FluentValidation, you create a class that inherits from the `AbstractValidator<T>` base class. Then, you can add the validation rules from the constructor using RuleFor:

```csharp
public record BreakfastDetails(string Name,
    string Description,
    DateTime StartDateTime,
    DateTime EndDateTime);
```

You would define a set of validation rules for this class by inheriting from `AbstractValidator<BreakfastDetails>`

The validation rules themselves should be defined in the validator class’s constructor.

To specify a validation rule for a particular property, call the RuleFor method, passing a lambda expression that indicates the property that you wish to validate.

```csharp
public class BreakfastDetailsValidator : AbstractValidator<BreakfastDetails>
{
    public BreakfastDetailsValidator()
    {
        RuleFor(a => a.Name).NotEmpty();
    }
}
```

### Complex Properties

Validators can be re-used for complex properties. For example, imagine you have two classes, `BreakfastDetails` and `CreateBreakfastRequest`:

```csharp
public record CreateBreakfastRequest(
    BreakfastDetails BreakfastDetails,
    List<string> Savory,
    List<string> Sweet);

public record BreakfastDetails(string Name,
    string Description,
    DateTime StartDateTime,
    DateTime EndDateTime);

```

Define an `BreakfastDetailsValidator`

```csharp
public class BreakfastDetailsValidator : AbstractValidator<BreakfastDetails>
{
    public BreakfastDetailsValidator()
    {
        RuleFor(a => a.Name).NotEmpty();
    }
}
```

Re-use the `BreakfastDetailsValidator` in the `CreateBreakfastRequestValidator` definition

```csharp
public class CreateBreakfastRequestValidator : AbstractValidator<CreateBreakfastRequest>
{
    public CreateBreakfastRequestValidator()
    {
        RuleFor(x => x.BreakfastDetails)
        .SetValidator(new BreakfastDetailsValidator());
    }
}
```

### Overriding the Message

You can override the default error message for a validator by calling the WithMessage method on a validator definition:

```csharp
 RuleFor(a => a.Name).NotEmpty()
 .WithMessage("Please ensure that you have entered your {{PropertyName}}")
```

### Message placeholders

The message can contain placeholders for special values such as `{PropertyName}` - which will be replaced at runtime. Each built-in validator has its own list of placeholders.

The placeholders used in all validators are:

- `{PropertyName}` – Name of the property being validated
- `{PropertyValue}` – Value of the property being validated These include the predicate validator (`Must` validator), the email and the regex validators.
  Used in comparison validators: (`Equal`, `NotEqual`, `GreaterThan`, `GreaterThanOrEqual`, etc.)

- `{ComparisonValue}` – Value that the property should be compared to
- `{ComparisonProperty}` – Name of the property being compared against (if any)
  Used only in the Length validator:

- `{MinLength}` – Minimum length
- `{MaxLength}` – Maximum length
- `{TotalLength}` – Number of characters entered

### Custom validators

There are several ways to create a custom, reusable validator. The recommended way is to make use of the **Predicate Validator** to write a custom validation function,

We create an extension method on `IRuleBuilder<T,TProperty>`, and we use a generic type constraint to ensure this method only appears in intellisense for DateTime types. Inside the method, we call the Must method in the same way as before but this time we call it on the passed-in RuleBuilder instance.

```csharp
public static class DateTimeValidators
{
    public static IRuleBuilderOptions<T, DateTime> AfterSunrise<T>(
      this IRuleBuilder<T, DateTime> ruleBuilder)
    {
        var sunrise = TimeOnly.MinValue.AddHours(6);

        return ruleBuilder
        .Must((objectRoot, dateTime, context) =>
        {
            var providedTime = TimeOnly.FromDateTime(dateTime);
            context.MessageFormatter.AppendArgument("Sunrise", sunrise);
            context.MessageFormatter.AppendArgument("ProvidedTime", providedTime);
            return providedTime > sunrise;
        }).WithMessage("{ProperyName} must be after {Sunrise}. Yor provided {ProvidedTime}");
    }
}
```

### Custom message placeholders

Overload of Must that we’re using now accepts 3 parameters: the `root` (parent) object, the property `value` itself, and the `context`. We use the context to add a custom message replacement value of **"Sunrise"** and set its value to the sunrise. We can now use this placeholder as `{Sunrise}` within the call to `WithMessage`.

### Register Validators

To automatically register all validators from an assembly, you need to call the AddValidatorsFromAssembly method:

```csharp
services.AddValidatorsFromAssemblyContaining<IAssemblyMarker>();
```
