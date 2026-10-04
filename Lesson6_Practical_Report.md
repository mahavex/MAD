# BBT 3.2 Mobile Application Development
## Lesson 6 Practical: Tenant Management Screen (Kotlin)

**Name:** ______________________  **Admission No:** ______________  **Date:** 4 October 2026

---

## 1. Objective
Build an Add Tenant screen that collects a tenant's name, phone number and rent paid, then displays the saved tenant. The screen is built in three stages: ConstraintLayout, View Binding, then Data Binding.

## 2. Project files
| File | Purpose |
|---|---|
| `activity_main.xml` | ConstraintLayout UI wrapped in `<layout>` with a `tenant` variable |
| `MainActivity.kt` | Reads the inputs, validates, creates a `Tenant`, hands it to the layout |
| `Tenant.kt` | Data class holding name, phone, rent, plus `summary()` |
| `build.gradle.kts (Module :app)` | Enables `viewBinding` and `dataBinding` |

## 3. How it works
1. **ConstraintLayout:** each view is positioned relative to its neighbours. `0dp` width means "match constraints", so the fields stretch between their start and end constraints. `numberDecimal` and `phone` input types show the number keyboard.
2. **View Binding:** with `viewBinding = true`, Android generates `ActivityMainBinding` from `activity_main.xml`. Views are reached as `binding.saveButton` instead of `findViewById()`, and a wrong ID is caught at compile time.
3. **Data Binding:** with `dataBinding = true` and the layout wrapped in `<layout>`, the XML declares `<variable name="tenant" type="...Tenant" />`. The expression `@{tenant.summary()}` displays the object. Data Binding is null-safe, so the grey hint shows until a tenant is saved.
4. **Separation of concerns:** `MainActivity` only creates the `Tenant`. How it is displayed lives in `Tenant.summary()` and the XML.

## 4. View Binding vs Data Binding
| | View Binding | Data Binding |
|---|---|---|
| Purpose | Kotlin reaches the views | XML reads data directly |
| Direction | Code to views | Data to views |
| Layout change | None | Wrapped in `<layout>` |
| Build speed | Fast | Slower |

## 5. Exercises ("Try it yourself")
1. **Empty-name validation:** `binding.tenantNameEditText.error = "Tenant name is required"` followed by `return@setOnClickListener`. `trim()` also rejects a name of only spaces.
2. **Rent text:** only `summary()` in `Tenant.kt` changed to `Rent paid: KSh ...`. `MainActivity.kt` was not touched, which shows the benefit of Data Binding.
3. **Second TextView:** `tenantNameResultTextView` uses `android:text="@{tenant.name}"`, 28sp, bold.
4. **Clear inputs:** after `binding.tenant = tenant`, each field is cleared with `.text.clear()` and focus returns to the name field. The saved tenant stays on screen because the result view displays the `Tenant` object, not the inputs.

## 6. Test results
| Test | Input | Expected result |
|---|---|---|
| App opens | none | Grey hint "Saved tenant will appear here" |
| Valid save | John Kamau / 0712345678 / 25000 | `Tenant: John Kamau`, `Phone: 0712345678`, `Rent paid: KSh 25000`, then "John Kamau" in large bold below; inputs cleared |
| Update | Mary Wanjiku / 0722000111 / 30000 | Result updates to Mary Wanjiku |
| Empty name | blank name, tap SAVE | Error "Tenant name is required"; nothing saved |

*Insert your emulator screenshots for each test here.*

## 7. Conclusion
The practical took one screen from static XML to a data-driven design. ConstraintLayout provides the interface, View Binding gives type-safe access to views, and Data Binding lets the layout display a `Tenant` object, which keeps the Activity small and the display logic reusable for the upcoming RecyclerView screen.

---

## Appendix: Source code
See `activity_main.xml`, `MainActivity.kt`, `Tenant.kt` and the Gradle snippet submitted with this report.
