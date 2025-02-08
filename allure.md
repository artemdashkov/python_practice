- `@allure.feature("Profile functionality")` - декоратор класса
- `@allure.title("Change profile name")`- декоратор для теста. 
- `@allure.severity("Critical")` - декоратор для теста.
- `@allure.step("Start test of the link...")`

```python
allure.dynamic.epic(dynamic_epic)
    allure.dynamic.feature(dynamic_feature)
    allure.dynamic.story(dynamic_story)
    allure.dynamic.title(dynamic_title)
```