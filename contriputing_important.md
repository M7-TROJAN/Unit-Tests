

# 🧠 الفكرة العامة الأول (مهم جدًا)

قبل أي أوامر Git، لازم تفهم الصورة الكبيرة:

### عندنا 3 مستويات:

1. ال **Local Machine (جهازك)**
2. ال **Git Repository (Local Git)**
3. ال **GitLab (Remote)**

و عندنا فروع (Branches) كل واحد له دور:

| Branch      | مين يشتغل عليه؟  | الهدف            |
| ----------- | ---------------- | ---------------- |
| `main`      | CI / Release فقط | Production       |
| `develop`   | Integrations     | تجميع الشغل      |
| `feature/*` | **إنت**          | شغلك اليومي      |
| `bugfix/*`  | **إنت**          | إصلاح Bugs       |
| `hotfix/*`  | Seniors          | مشاكل Production |

👉 **إنت عمرك ما تشتغل مباشرة على `develop`**

---

# 🧩 السيناريو الصح من البداية للنهاية

هنمشي سيناريو كامل كأنك بتشتغل Feature جديدة.

---

## 🟢 المرحلة 0: أول مرة تفتح المشروع (مرة واحدة بس)

### 1️⃣ Clone الريبو

```bash
git clone https://git.vmnova.com/vmnova/vmnova-production.git
cd vmnova-production
```

### 2️⃣ ظبط اسمك وإيميلك (مرة واحدة)

```bash
git config user.name "Mahmoud"
git config user.email "mahmoud@vmnova.com"
```

### 3️⃣ تأكد إنك على develop

```bash
git checkout develop
git pull origin develop
```

⚠️ **من اللحظة دي: develop ممنوع تشتغل عليه**

---

## 🟢 المرحلة 1: قبل ما تكتب سطر كود واحد

### ❓ جايلك شغل → لازم يكون له Issue

مثال:

```
VAP-321 - Add Myrtille Service
```

---

### ✅ تعمل Branch جديد

```bash
git checkout develop
git pull origin develop
git checkout -b feature/VAP-321-myrtille-service
```

🔑 **من اللحظة دي:**

* كل شغلك على البرانش ده
* ال develop في أمان
* المدير مرتاح 😄

---

## 🟢 المرحلة 2: تشتغل بقى (الكود)

### إنت تعمل:

* تضيف service
* تضيف projects
* تعدل solution
* تكتب كود
* أي حاجة

### تراجع شغلك:

```bash
git status
git diff
```

---

## 🟢 المرحلة 3: تعمل Commit (دي أخطر مرحلة)

### ❌ الغلط الشائع

* ال commit كبير فيه 100 تغيير
* رسالة commit مش مفهومة

### ✅ الصح

* ال Commit الواحد = فكرة واحدة

---

### 1️⃣ ال Stage التغييرات

```bash
git add .
```

أو:

```bash
git add src/Services/Myrtille
```

---

### 2️⃣ ال Commit بالصيغة الصح

حسب Contributing.md 👇

```bash
git commit -m "feat(myrtille): VAP-321: Add Myrtille service skeleton

- Added Domain, Application, Infrastructure, API projects
- Added service to solution
- Initial Program.cs and health endpoint
"
```

📌 ليه ده صح؟

* فيه `feat`
* فيه `scope`
* فيه `Issue ID`
* بيشرح *ليه* عملت كده

---

## 🟢 المرحلة 4: ترفع شغلك على GitLab

### أول Push

```bash
git push origin feature/VAP-321-myrtille-service
```

من هنا GitLab بقى شايف شغلك.

---

## 🟢 المرحلة 5: تعمل Merge Request (على GitLab)

### على GitLab:

1. **Merge Requests**
2. **New Merge Request**
3. Source:

   ```
   feature/VAP-321-myrtille-service
   ```
4. Target:

   ```
   develop
   ```

---

### تستخدم الـ Template الرسمي

```markdown
## VAP-321 - Add Myrtille Service

### Summary
Add initial Myrtille microservice following Clean Architecture.

### Changes Made
- Created Myrtille Domain, Application, Infrastructure, API
- Added health check endpoint
- Registered service in solution

### Type of Change
- [x] New feature

### Testing Performed
- [x] Build successful
- [ ] Unit tests (next MR)

Closes VAP-321
```

---

## 🟢 المرحلة 6: لو اتطلب منك تعديلات

المدير قال:

> عدّل كذا

### تعمل:

```bash
# تعدل الكود
git add .
git commit -m "VAP-321: Address review feedback

- Improved logging
- Fixed naming conventions
"
git push
```

⚠️ **ممنوع تعمل branch جديد**
⚠️ **ممنوع تعمل MR جديد**

---

## 🟢 المرحلة 7: بعد ما MR يتعمله Merge

### ترجع لجهازك:

```bash
git checkout develop
git pull origin develop
git branch -d feature/VAP-321-myrtille-service
```

ولو محتاج:

```bash
git push origin --delete feature/VAP-321-myrtille-service
```

---

# ❗ أخطاء لازم تتحفظ (Danger Zone)

### 🚫 متعملش:

* شغل مباشر على develop
* ال commit من غير Issue ID
* اي push على main
* ال commit باسم:

  ```
  update
  fix
  wip
  ```

---

# 🧠 خلاصة تحفظها عن ظهر قلب

> **Always:**

```
Issue → Branch → Code → Commit → Push → MR → Review → Merge
```

---

قولّي 💪
