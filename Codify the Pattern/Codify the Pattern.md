slidenumbers: false

# **Codify the Pattern**

## *How Swift lets us eschew convention*

---

# Objective-C Conventions in Swift

**Objective-C Convention** | **Swift**
---------------------------|------------------------------------
Initialization             | `convenience` & `designated`
Mutable / Immutable types  | `var` / `let`
nil, null, NSNull, -1      | Optional
Selectors                  | References to functions

---

```swift
func validateEmail(_ email: String) -> Bool {
    // Some check that the email is valid.
}
```

---

# Making Illegal States Unrepresentable

---

```swift
enum Result<T> {
    case success(value: T)
    case failure(error: Error)
}
```

---

# A customer must have at least one contact method

```swift
struct Customer {
    var phoneNumber: PhoneNumber?
    var emailAddress: EmailAddress?
}
```

Does this fulfill the business requirements?

---

#

```swift
struct Customer {

    enum ContactDetails {
        case phone(phone: PhoneNumber)
        case email(email: EmailAddress)
        case multiple(phone: PhoneNumber, email: EmailAddress)
    }

    var contactDetails: ContactDetails
}
```

---

# Initializers over convenient functions



---

# **Daniel Tull**
## *@danielctull*