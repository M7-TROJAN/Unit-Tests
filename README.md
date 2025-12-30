
# 🧪 يعني إيه Unit Testing أصلاً؟

**Unit Test** =
اختبار *أصغر وحدة منطقية* في الكود (Class / Method) **لوحدها**
من غير:

* داتابيز ❌
* Services حقيقية ❌
* Network ❌
* Windows Services ❌

يعني:

> نختبر القرار المنطقي… مش البيئة

---

## مثال بسيط

إنت عندك UseCase:

```csharp
EnsureMyrtilleInstalledUseCase
```

ده **مرشح مثالي** لـ Unit Testing لأنه:

* بياخد Interfaces
* مفيش static
* مفيش new جوة الكلاس
* كله Dependency Injection

---

# 🧠 المدير ليه قال لازم Unit Tests؟

لأن:

* يضمن إن logic التثبيت مش بيتكسر
* أي refactor بعد كده يبقى safe
* لو باظ Test → عرفنا فين المشكلة فورًا

---

# 🧩 هنستخدم إيه؟

## الأدوات القياسية في .NET

| الحاجة         | نستخدم                                  |
| -------------- | --------------------------------------- |
| Test Framework | **xUnit**                               |
| Mocking        | **Moq**                                 |
| Assertions     | **FluentAssertions** (اختياري بس ممتاز) |

---

# 1️⃣ إنشاء مشروع Unit Test

من الـ Solution:

```
Right click Solution
→ Add
→ New Project
→ xUnit Test Project
```

### الاسم (مهم):

```
VMNova.Myrtille.Application.Tests
```

---

## أضف References

* Reference لمشروع:

  * `VMNova.Myrtille.Application`
* NuGet Packages:

```bash
dotnet add package Moq
dotnet add package FluentAssertions
```

---

# 2️⃣ أول Test حقيقي على شغلك 🔥

هنبدأ بـ **EnsureMyrtilleInstalledUseCase**

---

## 🧪 Scenario 1

### "لو Myrtille سليمة → السيستم يبقى Ready فورًا"

---

### Arrange

```csharp
[Fact]
public async Task ExecuteAsync_WhenHealthy_ShouldMarkStateReady()
{
    // Arrange
    var inspection = new Mock<IMyrtilleInspectionService>();
    inspection.Setup(x => x.InspectAsync())
        .ReturnsAsync(MyrtilleHealthStatus.Healthy);

    var provider = new Mock<IMyrtilleInstallerProvider>();
    var state = new MyrtilleInstallationState();

    var logger = Mock.Of<ILogger<EnsureMyrtilleInstalledUseCase>>();
    var options = Options.Create(new MyrtilleOptions());

    var sut = new EnsureMyrtilleInstalledUseCase(
        logger,
        options.Value,
        state,
        inspection.Object,
        provider.Object
    );

    // Act
    await sut.ExecuteAsync(CancellationToken.None);

    // Assert
    state.Status.Should().Be(MyrtilleRuntimeStatus.Ready);
}
```

---

## ✨ بص حصل إيه؟

* Mockينا inspection
* قولناله: "ارجع Healthy"
* شغلنا UseCase
* اتأكدنا إن `state.MarkReady()` اتنادت

ده **Unit Test حقيقي 100%**

---

# 3️⃣ أهم Pattern تحفظه (احفظه زي اسمك)

## AAA Pattern

```csharp
// Arrange  → حضّر
// Act      → نفّذ
// Assert   → اتأكد
```

كل Test هتكتبه ماشي كده

---

# 4️⃣ ال Tests لازم تعملها في مشروعك

## 🔥ال  Minimum Required

### EnsureMyrtilleInstalledUseCase

* ✅ Healthy → Ready
* ✅ NotInstalled → Install called
* ✅ Unhealthy → Uninstall ثم Install
* ✅ Verification fails → throws exception
* ✅ Remote unreachable → throws

---

## Example: Unhealthy case

```csharp
provider.Verify(x => x.UninstallAsync(It.IsAny<CancellationToken>()), Times.Once);
provider.Verify(x => x.InstallAsync(It.IsAny<CancellationToken>()), Times.Once);
```

---

# 5️⃣ Test للـ InspectionService

ده أسهل:

```csharp
[Fact]
public async Task InspectAsync_WhenServicesNotInstalled_ShouldReturnNotInstalled()
{
    var provider = new Mock<IMyrtilleInstallerProvider>();

    provider.Setup(x => x.IsServiceInstalledAsync(It.IsAny<string>()))
        .ReturnsAsync(false);

    var options = Options.Create(new MyrtilleOptions());

    var sut = new MyrtilleInspectionService(provider.Object, options.Value);

    var result = await sut.InspectAsync();

    result.Should().Be(MyrtilleHealthStatus.NotInstalled);
}
```

---

# 6️⃣ حاجة مهمة جدًا تقولها لمديرك 🧠

> "احنا عاملين Unit Tests على Business Logic
> ومش بنعمل test للـ Windows Services نفسها"

وده الصح مهنيًا 👌

---

# 7️⃣ إنت كده مستواك إيه؟

بصراحة؟
من شكل الكود اللي إنت كاتبه:

> إنت مش Junior
> إنت **Mid-level** بس ناقصك Testing

وده طبيعي جدًا

---

## تحب نعمل إيه بعد كده؟

اختار:
1️⃣ نكتب **Test Plan** جاهز تبعته للمدير
2️⃣ نغطي كل UseCase عندك Test-by-Test
3️⃣ نشرح Moq بهدوء أكتر
4️⃣ نعمل Integration Tests بعد كده

قولّي وأنا معاك خطوة خطوة 💪
