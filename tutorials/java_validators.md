############################

############################
# JAVA VALIDATORS
############################

############################

# Validators

# "Bean Validation" version history

| version | RFC     |
|---------|---------|
| 1.0     | JSR-303 |
| 1.1     | JSR-349 |
| 2.0     | JSR 380 |

# Hibernate Validator 6.x
Is the reference implementation of "JSR 380 (or Bean Validation 2.0)".

Hibernate Validator offers additional value on top of the features required by Bean Validation. For example, a programmatic constraint configuration API as well as an annotation processor.

# Apache BVal
implementation of "JSR 380 (or Bean Validation 2.0)".

# Compability matrix

| Hibernate Validator     | 7.0     | 6.2     | 6.0     |
|-------------------------|---------|---------|---------|
| Java                    | 8 or 11 | 8 or 11 | 8 or 11 |
| Bean Validation         | N/A     | N/A     | 2.0     |
| Jakarta Bean Validation | 3.0     | 2.0     | N/A     |

# Spring Validation
Spring support validation with "org.springframework.validation.Validator" interface.

Spring has also optionally integration/support for "Bean Validation". "Bean Validation" is supporting also by adopting itself to "org.springframework.validation.Validator" interface.

example implementation for org.springframework.validation.Validator:

```java
public class PersonValidator implements org.springframework.validation.Validator {

    /**
     * This Validator validates *just* Person instances
     */
    public boolean supports(Class clazz) {
        return Person.class.equals(clazz);
    }

    public void validate(Object obj, org.springframework.validation.Errors e) {
        ValidationUtils.rejectIfEmpty(e, "name", "name.empty");
        Person p = (Person) obj;
        if (p.getAge() < 0) {
            e.rejectValue("age", "negativeValue");
        } else if (p.getAge() > 110) {
            e.rejectValue("age", "too.darn.old");
        }
    }
}
```

Validate programatically (manually):

```java
Person person = new Person();
person.setName("Jack");
BeanPropertyBindingResult result = new BeanPropertyBindingResult(person, "person");
ValidationUtils.invokeValidator(new PersonValidator(), person, result);
result.getAllErrors().stream().forEach(e ->
    System.out.println(messageSource.getMessage(e, Locale.US)));
```

Spring MVC integration (only using "Bean validation" annotations):

```java
@RequestMapping(value="/user", method=RequestMethod.POST)
public createUser(Model model, @Valid @ModelAttribute("user") User user, BindingResult result){
    if (result.hasErrors()){
      // do something
    }
    else {
      // do something else
    }
}
```

Spring MVC integration (only using "Bean validation" annotations):

```java
@RequestMapping(value="/user", method=RequestMethod.POST)
public createUser(Model model, @ModelAttribute("user") User user, BindingResult result){

    // exception will not throw until here! Because we did not use @Valid annotation.

    UserValidator userValidator = new UserValidator();
    userValidator.validate(user, result);

    if (result.hasErrors()){
      // do something
    }
    else {
      // do something else
    }
}
```

# JSR 380 (or Bean Validation 2.0)
requires min Java 8. supports types like Optional and LocalDate.

Dependencies:

Only API (javax.validation.* packages):

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-API</artifactId>
    <version>2.0.1.Final</version>
</dependency>
```

Reference Implementation:

```xml
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.13.Final</version>
</dependency>
```

Example:

```java
import javax.validation.constraints.AssertTrue;
import javax.validation.constraints.Max;
import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
import javax.validation.constraints.Email;

public class User {

    @NotNull(message = "Name cannot be null")
    private String name;

    @AssertTrue
    private boolean working;

    @Size(min = 10, max = 200, message
      = "About Me must be between 10 and 200 characters")
    private String aboutMe;

    @Min(value = 18, message = "Age should not be less than 18")
    @Max(value = 150, message = "Age should not be greater than 150")
    private int age;

    @Email(message = "Email should be valid")
    private String email;

    // standard setters and getters
}
```

Some other features:

```java
List<@NotBlank String> preferences;
```

```java
public Optional<@Past LocalDate> getDateOfBirth() {
    return Optional.of(dateOfBirth);
}
```

Spring integration (programatically validation):

```java
ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
Validator validator = factory.getValidator();

User user = new User();
user.setWorking(true);
user.setAboutMe("Its all about me!");
user.setAge(50);

Set<ConstraintViolation<User>> violations = validator.validate(user);

for (ConstraintViolation<User> violation : violations) {
    log.error(violation.getMessage() +  violation.getPropertyPath());
}
```