
`@AuthenticationPrincipal` is a Spring Security annotation used to directly inject the currently authenticated user's principal object into a controller method.

It eliminates the need to manually retrieve the user from the `SecurityContextHolder`.

### Why Use It

Without `@AuthenticationPrincipal`, developers usually need to access the security context manually:

```java
Authentication authentication =
        SecurityContextHolder.getContext().getAuthentication();

CustomUserDetails user =
        (CustomUserDetails) authentication.getPrincipal();
````

Using `@AuthenticationPrincipal`, Spring Security automatically extracts the principal and injects it into the method parameter:

```java
@GetMapping("/profile")
public UserProfile getProfile(
        @AuthenticationPrincipal CustomUserDetails user) {

    return userService.getProfile(user.getUserId());
}
```

This makes the code cleaner and more readable.

---

### **How It Works**

Authentication information is stored in the Spring Security context after successful login.

```text
SecurityContextHolder
    └── SecurityContext
            └── Authentication
                    └── Principal
```

When Spring processes a request:

1. The Security Filter Chain authenticates the user.
2. An `Authentication` object is created.
3. The authenticated user object is stored as the `Principal`.
4. `@AuthenticationPrincipal` extracts the principal and injects it into the controller method.

---

### **Common Example**

#### **Custom UserDetails**

```java
public class CustomUserDetails implements UserDetails {

    private Long userId;
    private String username;

    // getters
}
```

#### **Controller**

```java
@RestController
@RequestMapping("/user")
public class UserController {

    @GetMapping("/info")
    public UserInfo getUserInfo(
            @AuthenticationPrincipal CustomUserDetails user) {

        return userService.getUserInfo(user.getUserId());
    }
}
```

After login:

```java
{
    "userId": 1001,
    "username": "jasper"
}
```

Spring automatically injects the `CustomUserDetails` object into the `user` parameter.

---

### **Accessing JWT User Information**

A very common use case is JWT authentication.

Suppose a JWT filter parses the token and creates an authentication object:

```java
UsernamePasswordAuthenticationToken authentication =
        new UsernamePasswordAuthenticationToken(
                userDetails,
                null,
                userDetails.getAuthorities());

SecurityContextHolder.getContext()
                     .setAuthentication(authentication);
```

Then the controller can directly obtain the authenticated user:

```java
@GetMapping("/current")
public UserVO currentUser(
        @AuthenticationPrincipal CustomUserDetails user) {

    return UserVO.builder()
            .userId(user.getUserId())
            .username(user.getUsername())
            .build();
}
```

No manual token parsing is required inside the controller.

---

### **Using Expression Extraction**

`@AuthenticationPrincipal` supports SpEL expressions.

If the principal object contains a nested field:

```java
public class CustomUserDetails {

    private User user;
}
```

You can extract the nested object directly:

```java
@GetMapping("/info")
public User getUser(
        @AuthenticationPrincipal(expression = "user") User user) {

    return user;
}
```

Equivalent to:

```java
CustomUserDetails principal = ...;
User user = principal.getUser();
```

---

### **Difference Between Principal and Authentication**

#### **Principal**

Represents the currently logged-in user.

```java
@AuthenticationPrincipal
CustomUserDetails user
```

Equivalent to:

```java
authentication.getPrincipal();
```

---

#### **Authentication**

Represents the complete authentication information.

```java
@GetMapping("/test")
public String test(Authentication authentication) {

    return authentication.getName();
}
```

Contains:

- Principal
- Credentials
- Authorities
- Authentication Status

Structure:

```text
Authentication
├── Principal
├── Credentials
├── Authorities
└── Details
```

---

### **@AuthenticationPrincipal vs SecurityContextHolder**

#### **Using SecurityContextHolder**

```java
Authentication authentication =
        SecurityContextHolder.getContext().getAuthentication();

CustomUserDetails user =
        (CustomUserDetails) authentication.getPrincipal();
```

Advantages:

- Can be used anywhere.
- Suitable for Service or Utility classes.

Disadvantages:

- More boilerplate code.
- Requires type casting.

---

#### **Using @AuthenticationPrincipal**

```java
@GetMapping("/profile")
public Profile getProfile(
        @AuthenticationPrincipal CustomUserDetails user) {

    return profileService.getProfile(user.getUserId());
}
```

Advantages:

- Cleaner code.
- No manual casting.
- Recommended for Controller layer.

Disadvantages:

- Only available in Spring-managed web request methods.

---

### **Typical Usage Scenarios**

#### **Current User Information**

```java
@GetMapping("/me")
public UserVO me(
        @AuthenticationPrincipal CustomUserDetails user) {

    return userMapper.toVO(user);
}
```

#### **User-Owned Resource Query**

```java
@GetMapping("/orders")
public List<OrderVO> orders(
        @AuthenticationPrincipal CustomUserDetails user) {

    return orderService.findByUserId(user.getUserId());
}
```

#### **User-Owned Resource Update**

```java
@PostMapping("/profile")
public void updateProfile(
        @AuthenticationPrincipal CustomUserDetails user,
        @RequestBody ProfileUpdateRequest request) {

    profileService.update(user.getUserId(), request);
}
```

---

### **Best Practices**

1. Use `@AuthenticationPrincipal` in the Controller layer whenever possible.
2. Store a custom `UserDetails` implementation in the Authentication object.
3. Avoid repeatedly calling `SecurityContextHolder` in controller methods.
4. Pass only necessary user information into the Service layer.
5. In JWT-based systems, populate the Authentication object once in the filter and reuse it throughout the request lifecycle.

---

### **Summary**

`@AuthenticationPrincipal` is a Spring Security annotation that injects the current authenticated user’s principal object directly into controller method parameters.

Core purpose:

```text
Authentication
    └── Principal
           ↓
@AuthenticationPrincipal
```

It is essentially a convenient shortcut for:

```java
SecurityContextHolder
    .getContext()
    .getAuthentication()
    .getPrincipal();
```

and is the preferred way to obtain the current logged-in user in Spring MVC and Spring Security controllers.