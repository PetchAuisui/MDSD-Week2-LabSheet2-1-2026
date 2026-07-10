# ใบงานการทดลองที่ 2-1
# Dart Programming Fundamentals
### วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่

| | |
|--|--|
| **สัปดาห์ที่** | 2 |
| **ใบงานที่** | 2-1 จาก 2 |
| **เวลา** | 2 ชั่วโมง 30 นาที |
| **เครื่องมือ** | DartPad (dartpad.dev) |

---

## วัตถุประสงค์

เมื่อสิ้นสุดการทดลอง นักศึกษาสามารถ:

1. เขียนโปรแกรม Dart ที่มี Type System, Null Safety, Collection ได้ถูกต้อง
2. ออกแบบและเขียน Function ทั้งแบบ Named Parameter, Optional Parameter และ Higher-order Function ได้
3. ออกแบบ Class ที่มี Constructor, Getter, Inheritance, Abstract Class และ Mixin ได้
4. เขียนโปรแกรม Async ด้วย Future, async/await และ Future.wait ได้ พร้อมจัดการ Error
5. อ่าน Error Message ของ Dart และแก้ไขได้ด้วยตนเอง

---

## การเตรียมตัวก่อนทดลอง

ใบงานนี้ใช้ **DartPad** ทั้งหมด ไม่จำเป็นต้องติดตั้งโปรแกรมใดๆ เพิ่มเติม

1. เปิด Browser (แนะนำ Chrome หรือ Edge)
2. ไปที่ **https://dartpad.dev**
3. ตรวจสอบว่าหน้าตาเป็นดังนี้

```
┌─────────────────────────────────────────────────────┐
│  DartPad                              [Dart ▼] [Run]│
├───────────────────────────┬─────────────────────────┤
│                           │                         │
│   บริเวณเขียนโค้ด            │   บริเวณแสดงผล           │
│   (Code Editor)           │   (Console Output)      │
│                           │                         │
└───────────────────────────┴─────────────────────────┘
```

> **หมายเหตุ:** ตรวจสอบว่าเลือก **Dart** (ไม่ใช่ Flutter) ที่ Dropdown มุมบนขวา ก่อนเริ่มทุกการทดลอง

---

## ส่วนที่ 1 — ทฤษฎีและการทดลอง: Variables, Types และ Null Safety

### ทฤษฎี 1.1 — ระบบชนิดข้อมูล (Type System)

Dart เป็นภาษา **Strongly Typed** หมายความว่าทุกตัวแปรมีชนิดข้อมูลที่แน่นอน และชนิดนั้นไม่เปลี่ยนตลอดอายุการใช้งาน ซึ่งต่างจากภาษา Dynamic เช่น Python ที่ตัวแปรเดียวกันสามารถเป็นได้ทั้ง String และ Number

**ชนิดข้อมูลพื้นฐานใน Dart:**

| ชนิด | ความหมาย | ตัวอย่างค่า |
|------|----------|-----------|
| `int` | จำนวนเต็ม | `0`, `42`, `-10` |
| `double` | จำนวนทศนิยม | `3.14`, `2.0`, `-0.5` |
| `String` | ข้อความ | `"สวัสดี"`, `'hello'` |
| `bool` | ค่าจริง/เท็จ | `true`, `false` |
| `List<T>` | รายการ (Array) | `[1, 2, 3]` |
| `Map<K,V>` | คู่ Key-Value | `{"name": "ชาย"}` |
| `Set<T>` | ชุดไม่ซ้ำ | `{1, 2, 3}` |

**การประกาศตัวแปร:**

```dart
// แบบที่ 1: ระบุ Type ชัดเจน — อ่านง่าย รู้ Type ทันที
String name = "สมชาย";
int age = 20;
double gpa = 3.75;

// แบบที่ 2: ใช้ var — Dart อนุมาน Type จากค่าที่กำหนด (Type Inference)
var city = "กรุงเทพฯ";  // Dart รู้ว่าเป็น String
var score = 95;           // Dart รู้ว่าเป็น int

// แบบที่ 3: final — กำหนดค่าได้ครั้งเดียว (Runtime Constant)
final birthYear = 2004;    // เปลี่ยนหลังกำหนดไม่ได้
final now = DateTime.now(); // ค่าถูกกำหนด ณ Runtime

// แบบที่ 4: const — ค่าคงที่ Compile Time (ต้องรู้ค่าก่อนรัน)
const pi = 3.14159;
const maxScore = 100;
// const now = DateTime.now(); ← ❌ Error! DateTime.now() ไม่รู้ก่อนรัน
```

**String Interpolation** — การฝังตัวแปรใน String:

```dart
String name = "สมชาย";
int age = 20;

// วิธีที่ 1: $variable — ใช้กับตัวแปรเดี่ยว
print("ชื่อ: $name");           // → ชื่อ: สมชาย

// วิธีที่ 2: ${expression} — ใช้กับ Expression ที่ซับซ้อน
print("อายุ: ${age} ปี");       // → อายุ: 20 ปี
print("ปีเกิด: ${2025 - age}"); // → ปีเกิด: 2005
print("ชื่อ: ${name.toUpperCase()}"); // → ชื่อ: สมชาย (ตัวพิมพ์ใหญ่)
```

---

### ทฤษฎี 1.2 — Null Safety

ก่อนที่ Dart จะมี Null Safety โปรแกรมเมอร์มักพบ Error ที่ทำให้แอป Crash แบบนี้:

```
Unhandled Exception: Null check operator used on a null value
```

Dart 2.12+ แก้ปัญหานี้ด้วยระบบ **Sound Null Safety** — Compiler จะ**ไม่ยอม**ให้โค้ดที่อาจเกิด Null Error ผ่านได้

```
ตัวแปร Dart แบ่งเป็น 2 ประเภท:

Non-nullable (default):          Nullable (เพิ่ม ?):
┌─────────────────────┐         ┌─────────────────────┐
│  String name        │         │  String? nickname   │
│  ┌───────────────┐  │         │  ┌───────────────┐  │
│  │ ต้องมีค่า        │  │         │  │ มีค่า หรือ       │  │
│  │ เสมอ!         │  │         │  │ null ก็ได้      │  │
│  └───────────────┘  │         │  └───────────────┘  │
└─────────────────────┘         └─────────────────────┘
```

**Null-aware Operators — เครื่องมือจัดการ Null อย่างปลอดภัย:**

```dart
String? nickname = null;

// 1. ?? (Null Coalescing) — "ถ้า null ให้ใช้ค่าขวาแทน"
String display = nickname ?? "ไม่มีชื่อเล่น";
print(display); // → ไม่มีชื่อเล่น

// 2. ?. (Null-aware method call) — "เรียก method เฉพาะเมื่อไม่ null"
int? length = nickname?.length;
print(length);  // → null (ไม่ Crash)

// 3. ??= (Null-aware assignment) — "กำหนดค่าเฉพาะถ้าตัวแปรเป็น null"
nickname ??= "ชื่อเล่นเริ่มต้น";
print(nickname); // → ชื่อเล่นเริ่มต้น

// 4. ! (Null Assertion) — "ฉันมั่นใจว่าไม่ null" ⚠️ ใช้ด้วยความระวัง
String definitelyNotNull = "ค่าจริง";
String? maybeNull = definitelyNotNull;
print(maybeNull!.length); // → 8 (ถ้าผิดพลาดและเป็น null จะ Crash)
```

**Collections:**

```dart
// List — รายการที่มีลำดับ เพิ่ม/ลบได้
List<String> fruits = ["แอปเปิล", "กล้วย", "ส้ม"];
fruits.add("มะม่วง");
fruits.remove("กล้วย");
print(fruits.length);    // → 3
print(fruits[0]);        // → แอปเปิล
print(fruits.first);     // → แอปเปิล
print(fruits.last);      // → ส้ม

// Map — คู่ Key-Value
Map<String, int> scores = {
  "คณิตศาสตร์": 85,
  "วิทยาศาสตร์": 92,
  "ภาษาไทย": 78,
};
scores["ภาษาอังกฤษ"] = 88;  // เพิ่ม entry ใหม่
print(scores["คณิตศาสตร์"]); // → 85
print(scores["ชีววิทยา"]);   // → null (ไม่มี Key นี้)

// วนซ้ำใน Map
scores.forEach((subject, score) {
  print("$subject: $score");
});

// Set — ชุดข้อมูลที่ไม่มีซ้ำ
Set<String> tags = {"dart", "flutter", "mobile"};
tags.add("dart");  // ไม่เพิ่ม เพราะซ้ำอยู่แล้ว
print(tags.length); // → 3
```

---

### การทดลอง 1.1 — Variables, Types และ Collections

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** เปิด dartpad.dev เลือก Dart แล้วล้างโค้ดเดิมออก

**ขั้นตอนที่ 2** พิมพ์โค้ดต่อไปนี้ทีละบล็อก อ่านทำความเข้าใจก่อนกด Run

```dart
void main() {
  // === บล็อกที่ 1: ชนิดข้อมูลพื้นฐาน ===
  String studentName = "สมชาย ดีใจ";
  int studentAge = 20;
  double gpa = 3.75;
  bool isEnrolled = true;

  print("=== ข้อมูลนักศึกษา ===");
  print("ชื่อ: $studentName");
  print("อายุ: $studentAge ปี");
  print("GPA: $gpa");
  print("ลงทะเบียนแล้ว: $isEnrolled");
  print("ปีเกิด (ประมาณ): ${2026 - studentAge}");
}
```

**ขั้นตอนที่ 3** กด **Run** ตรวจสอบผลลัพธ์ที่ได้
- Screenshot
  <img width="1470" height="920" alt="image" src="https://github.com/user-attachments/assets/6365cf4a-8579-448e-a17d-64e1afa9d421" />

**ขั้นตอนที่ 4** เพิ่มโค้ดต่อไปนี้ **ต่อท้าย** ภายใน `main()` ก่อนปิด `}`

```dart
  // === บล็อกที่ 2: Null Safety ===
  print("\n=== Null Safety ===");
  String? nickname = null;
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ไม่มี

  nickname = "ชาย";
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ชาย
  print("ความยาว: ${nickname?.length}");       // → 3
  print("ตัวพิมพ์ใหญ่: ${nickname?.toUpperCase()}"); // → ชาย
```

**ขั้นตอนที่ 5** กด Run อีกครั้ง สังเกตผลลัพธ์ที่เพิ่มขึ้น
- Screenshot
  <img width="1470" height="920" alt="image" src="https://github.com/user-attachments/assets/d4210cb4-0f43-4425-91eb-4931765a5b31" />

**ขั้นตอนที่ 6** เพิ่มโค้ด Collections ต่อท้าย

```dart
  // === บล็อกที่ 3: List ===
  print("\n=== รายวิชาที่ลงทะเบียน ===");
  List<String> courses = ["Mobile Dev", "Web Dev", "AI"];
  Map<String, int> courseScores = {
    "Mobile Dev": 90,
    "Web Dev": 85,
    "AI": 92,
  };

  // วนซ้ำแสดงรายวิชาและคะแนน
  for (int i = 0; i < courses.length; i++) {
    String course = courses[i];
    int? score = courseScores[course];
    print("${i + 1}. $course: ${score ?? 'ยังไม่มีคะแนน'} คะแนน");
  }

  // คำนวณเฉลี่ย
  int total = courseScores.values.reduce((a, b) => a + b);
  double avg = total / courseScores.length;
  print("คะแนนเฉลี่ย: ${avg.toStringAsFixed(2)}");
```

**ขั้นตอนที่ 7** กด Run และบันทึกผลลัพธ์ทั้งหมด <br>
- Screenshot
  <img width="1470" height="922" alt="image" src="https://github.com/user-attachments/assets/d9823d38-5728-4ba6-bfd7-74e539d80ee4" />


---

### 🎯 โจทย์ฝึกทำ 1.1 — แก้ไขและเพิ่มเติมโค้ดด้วยตนเอง

แก้ไขโค้ดที่มีอยู่ให้ทำสิ่งต่อไปนี้ได้ครบ:

1. เพิ่มรายวิชา "Database" ที่มีคะแนน 88 ลงใน `courses` และ `courseScores`
2. หาวิชาที่มีคะแนนสูงสุดโดยใช้ `courseScores.entries` และ `.reduce()` แล้วพิมพ์ว่า "วิชาที่ได้คะแนนสูงสุด: ..."
3. นับจำนวนวิชาที่ได้คะแนน >= 90 แล้วพิมพ์ผล
4. สร้าง `Set<String>` ชื่อ `passedCourses` ที่เก็บเฉพาะวิชาที่ได้คะแนน >= 80 แล้วพิมพ์รายการ

**ผลลัพธ์ที่คาดหวัง (ตัวอย่าง):**
```
วิชาที่ได้คะแนนสูงสุด: AI (92 คะแนน)
จำนวนวิชาที่ได้ >= 90: 2 วิชา
วิชาที่ผ่าน: {Mobile Dev, Web Dev, AI, Database}
```
**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
void main() {

  // === บล็อกที่ 1: ชนิดข้อมูลพื้นฐาน ===
  String studentName = "ศิวาภัทร อุยสุย";
  int studentAge = 21;
  double gpa = 3.98;
  bool isEnrolled = true;

  print("=== ข้อมูลนักศึกษา ===");
  print("ชื่อ: $studentName");
  print("อายุ: $studentAge ปี");
  print("GPA: $gpa");
  print("ลงทะเบียนแล้ว: $isEnrolled");
  print("ปีเกิด (ประมาณ): ${2026 - studentAge}");
  
    // === บล็อกที่ 2: Null Safety ===
  print("\n=== Null Safety ===");
  String? nickname = null;
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ไม่มี

  nickname = "เพชร";
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ชาย
  print("ความยาว: ${nickname?.length}");       // → 3
  print("ตัวพิมพ์ใหญ่: ${nickname?.toUpperCase()}");// → ชาย
  
    // === บล็อกที่ 3: List ===
  print("\n=== รายวิชาที่ลงทะเบียน ===");
  List<String> courses = ["Database","Mobile Dev", "Web Dev", "AI"];
  Map<String, int> courseScores = {
    "Database" : 88,
    "Mobile Dev": 90,
    "Web Dev": 85,
    "AI": 92,
  };

  // วนซ้ำแสดงรายวิชาและคะแนน
  for (int i = 0; i < courses.length; i++) {
    String course = courses[i];
    int? score = courseScores[course];
    print("${i + 1}. $course: ${score ?? 'ยังไม่มีคะแนน'} คะแนน");
  }

  // คำนวณเฉลี่ย
  int total = courseScores.values.reduce((a, b) => a + b);
  double avg = total / courseScores.length;
  print("คะแนนเฉลี่ย: ${avg.toStringAsFixed(2)}");
  
  var highestCourse = courseScores.entries.reduce((current,next) => current.value > next.value ? current : next);
  print("วิชาที่ได้คะแนนสูงที่สุด : ${highestCourse.key} (${highestCourse.value} คะแนน)");
 
  int count = courseScores.values.where((score) => score >= 90).length;
  print("จำนวนวิชาที่ได้คะแนนตั้งแต่ 90 คะแนนขึ้นไป: $count วิชา");
  
  Set<String> passedCourses = {};
  courseScores.forEach((course,score) {
    if (score >= 80) {
      passedCourses.add(course);
    }
  });
  print("วิชาที่คะแนนผ่านเกณฑ์(เกณฑ์ผ่านคือ 80 คะแนน): $passedCourses");
  
}
```
**ScreenShot**
  <img width="1470" height="922" alt="image" src="https://github.com/user-attachments/assets/76ece463-4ead-47c9-817d-322f5e52a89d" />

---

## ส่วนที่ 2 — ทฤษฎีและการทดลอง: Functions

### ทฤษฎี 2.1 — รูปแบบ Function ใน Dart

Function คือหน่วยของโค้ดที่แยกออกมาเพื่อทำงานเฉพาะอย่าง ช่วยให้ไม่ต้องเขียนโค้ดซ้ำ

**รูปแบบ Function พื้นฐาน:**

```dart
// โครงสร้าง: ReturnType functionName(ParameterType paramName) { ... }

// Function ที่คืนค่า String
String greet(String name) {
  return "สวัสดี $name!";
}

// Function ที่ไม่คืนค่า (void)
void printDivider(int length) {
  print("─" * length);
}

// Arrow Function: ย่อเมื่อ body มีแค่ return expression เดียว
String greetArrow(String name) => "สวัสดี $name!";
double square(double x) => x * x;
bool isAdult(int age) => age >= 18;
```

**Positional Parameters — ต้องส่งตามลำดับ:**

```dart
// Required positional — ต้องส่งครบทุกตัว
double calculateBMI(double weight, double height) {
  return weight / (height * height);
}
print(calculateBMI(70, 1.75)); // → 22.86 ✅
// print(calculateBMI(1.75, 70)); // ✅ รัน แต่ผิดความหมาย!

// Optional positional — ใส่ [] รอบ Parameter ที่ไม่บังคับ
String formatName(String firstName, [String? lastName, String title = ""]) {
  if (lastName != null) {
    return "$title $firstName $lastName".trim();
  }
  return "$title $firstName".trim();
}
print(formatName("สมชาย"));              // → สมชาย
print(formatName("สมชาย", "ดีใจ"));      // → สมชาย ดีใจ
print(formatName("สมชาย", "ดีใจ", "นาย")); // → นาย สมชาย ดีใจ
```

**Named Parameters — ต้องระบุชื่อเมื่อเรียก:**

```dart
// Named parameters ใส่ {} รอบ — ลำดับไม่สำคัญ
void createProfile({
  required String name,    // required = บังคับส่ง
  required String email,   // required = บังคับส่ง
  int age = 0,             // optional + default value
  String? bio,             // optional nullable
}) {
  print("ชื่อ: $name");
  print("Email: $email");
  print("อายุ: $age ปี");
  if (bio != null) print("Bio: $bio");
}

// เรียกใช้ — ไม่ต้องสนใจลำดับ แต่ต้องระบุชื่อ
createProfile(
  name: "สมชาย",
  email: "somchai@example.com",
  age: 20,
  bio: "นักศึกษา KMITL",
);

createProfile(
  email: "guest@example.com",  // ลำดับต่างกันก็ได้
  name: "ผู้เยี่ยมชม",
  // age ไม่ส่งก็ได้ มีค่า default เป็น 0
);
```

---

### ทฤษฎี 2.2 — Higher-Order Functions

**Higher-order Function** คือ Function ที่รับ Function อื่นเป็น Parameter หรือคืนค่าเป็น Function ซึ่งทำให้เขียนโค้ดที่ยืดหยุ่นและกระชับมากขึ้น

```
แนวคิด:
ปกติเราส่ง ตัวเลข, ข้อความ เป็น Parameter
Higher-order Function ส่ง Function เป็น Parameter ได้ด้วย!

f(x) = x * 2          ← Function ธรรมดา
g(f, x) = f(x) + 1    ← Higher-order Function (รับ f เป็น parameter)
```

**Collection Methods ที่ใช้บ่อย:**

```dart
List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// where() — กรองรายการตามเงื่อนไข (คืน Iterable)
var evens = numbers.where((n) => n % 2 == 0).toList();
print(evens); // → [2, 4, 6, 8, 10]

// map() — แปลงทุก element (คืน Iterable ของ Type ใหม่)
var doubled = numbers.map((n) => n * 2).toList();
print(doubled); // → [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

var asString = numbers.map((n) => "No.$n").toList();
print(asString); // → [No.1, No.2, ...]

// reduce() — รวมทั้งหมดเป็นค่าเดียว
int sum = numbers.reduce((acc, n) => acc + n);
print(sum); // → 55

int max = numbers.reduce((a, b) => a > b ? a : b);
print(max); // → 10

// any() — มี element ใดสอดคล้องเงื่อนไขไหม?
bool hasNegative = numbers.any((n) => n < 0);
print(hasNegative); // → false

// every() — ทุก element สอดคล้องเงื่อนไขไหม?
bool allPositive = numbers.every((n) => n > 0);
print(allPositive); // → true

// sort() — เรียงลำดับ (แก้ List เดิม)
List<int> scores = [85, 42, 96, 71, 58];
scores.sort((a, b) => b.compareTo(a)); // เรียงจากมากไปน้อย
print(scores); // → [96, 85, 71, 58, 42]
```

**Function เป็น Variable:**

```dart
// เก็บ Function ในตัวแปร
String Function(String) shout = (s) => s.toUpperCase() + "!!!";
print(shout("hello")); // → HELLO!!!

// ส่ง Function เป็น Parameter
void applyToList(List<int> list, void Function(int) action) {
  for (var item in list) {
    action(item);
  }
}

applyToList([1, 2, 3], (n) => print("ค่า: $n"));
// → ค่า: 1
// → ค่า: 2
// → ค่า: 3
```

---

### การทดลอง 2.1 — Functions พื้นฐาน

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** ล้างโค้ดใน DartPad แล้วพิมพ์โค้ดต่อไปนี้

```dart
// === Function พื้นฐาน ===
String gradeLabel(double gpa) {
  if (gpa >= 3.5) return "เกียรตินิยมอันดับ 1";
  if (gpa >= 3.25) return "เกียรตินิยมอันดับ 2";
  if (gpa >= 3.0) return "ดีมาก";
  if (gpa >= 2.5) return "ดี";
  if (gpa >= 2.0) return "พอใช้";
  return "ต่ำกว่าเกณฑ์";
}

// === Arrow Function ===
double average(List<double> nums) =>
    nums.reduce((a, b) => a + b) / nums.length;

bool isHonors(double gpa) => gpa >= 3.25;

// === Named Parameters ===
void printStudent({
  required String name,
  required double gpa,
  int year = 1,
  String? major,
}) {
  print("─────────────────────────");
  print("ชื่อ: $name (ปีที่ $year)");
  if (major != null) print("สาขา: $major");
  print("GPA: $gpa → ${gradeLabel(gpa)}");
  print("เกียรตินิยม: ${isHonors(gpa) ? "✅ ใช่" : "❌ ไม่"}");
}

void main() {
  print("=== รายชื่อนักศึกษา ===\n");

  printStudent(name: "สมชาย", gpa: 3.75, year: 3, major: "เทคโนโลยีคอมพิวเตอร์");
  printStudent(name: "สมหญิง", gpa: 2.90, year: 2);
  printStudent(name: "สมศักดิ์", gpa: 3.30, year: 4, major: "เทคโนโลยีคอมพิวเตอร์");

  print("\n=== GPA เฉลี่ยทั้งชั้น ===");
  List<double> allGpas = [3.75, 2.90, 3.30];
  print("เฉลี่ย: ${average(allGpas).toStringAsFixed(2)}");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตผลลัพธ์
- Screenshot
  <img width="1470" height="920" alt="image" src="https://github.com/user-attachments/assets/85facf60-e0a6-4f22-be28-923df07f47ef" />

**ขั้นตอนที่ 3** ทดลองเปลี่ยนค่า `gpa` ในแต่ละ `printStudent()` เพื่อดูว่า Label เปลี่ยนอย่างไร
- Screenshot
  <img width="1470" height="956" alt="ภาพถ่ายหน้าจอ 2569-07-10 เวลา 23 34 01" src="https://github.com/user-attachments/assets/30292f3f-daef-407a-9af4-15d2dfde1eda" />

---

### การทดลอง 2.2 — Higher-Order Functions

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดต่อไปนี้

```dart
void main() {
  List<Map<String, dynamic>> students = [
    {"name": "สมชาย",  "gpa": 3.75, "year": 3, "faculty": "วิศวกรรม"},
    {"name": "สมหญิง", "gpa": 2.50, "year": 1, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศักดิ์","gpa": 3.10, "year": 2, "faculty": "วิศวกรรม"},
    {"name": "สมใจ",  "gpa": 1.80, "year": 4, "faculty": "บริหาร"},
    {"name": "สมปอง", "gpa": 3.50, "year": 2, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศรี", "gpa": 2.90, "year": 3, "faculty": "บริหาร"},
  ];

  // === where() — กรองนักศึกษาที่ GPA >= 3.0 ===
  print("=== นักศึกษาที่ GPA >= 3.0 ===");
  var honorStudents = students
      .where((s) => (s["gpa"] as double) >= 3.0)
      .toList();
  for (var s in honorStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }

  // === map() — แปลงเป็น String รายงาน ===
  print("\n=== รายงานนักศึกษา ===");
  var report = students
      .map((s) => "${s["name"]} (${s["faculty"]}) GPA: ${s["gpa"]}")
      .toList();
  report.forEach(print);

  // === sort() + reduce() ===
  print("\n=== วิเคราะห์คะแนน ===");
  List<double> gpas = students.map((s) => s["gpa"] as double).toList();

  double maxGpa = gpas.reduce((a, b) => a > b ? a : b);
  double minGpa = gpas.reduce((a, b) => a < b ? a : b);
  double avgGpa = gpas.reduce((a, b) => a + b) / gpas.length;

  print("GPA สูงสุด: $maxGpa");
  print("GPA ต่ำสุด: $minGpa");
  print("GPA เฉลี่ย: ${avgGpa.toStringAsFixed(2)}");

  // === any() และ every() ===
  bool anyFailing = students.any((s) => (s["gpa"] as double) < 2.0);
  bool allPassing = students.every((s) => (s["gpa"] as double) >= 2.0);
  print("มีนักศึกษาที่ GPA < 2.0: $anyFailing");
  print("ทุกคน GPA >= 2.0: $allPassing");
}
```

**ขั้นตอนที่ 2** กด Run และอ่านผลลัพธ์ทุกส่วน
- Screenshot
  <img width="1470" height="920" alt="image" src="https://github.com/user-attachments/assets/1293cb70-b227-44ec-9105-db723cbea3df" />

**ขั้นตอนที่ 3** เพิ่มโค้ดต่อท้ายใน `main()` เพื่อกรองนักศึกษาเฉพาะคณะ "วิศวกรรม" แล้วแสดงผล

```dart
  // ทดลองเพิ่มเอง: กรองเฉพาะคณะวิศวกรรม
  print("\n=== นักศึกษาคณะวิศวกรรม ===");
  var engineeringStudents = students
      .where((s) => s["faculty"] == "วิศวกรรม")
      .toList();
  // พิมพ์ชื่อและ GPA ของนักศึกษาแต่ละคน
  for (var s in engineeringStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }
```
- Screenshot
  <img width="1470" height="923" alt="image" src="https://github.com/user-attachments/assets/13b07e6d-e339-482c-aa63-a12a592eb5c1" />


---

### 🎯 โจทย์ฝึกทำ 2 — เขียน Function ด้วยตนเอง

เขียนโค้ดโดยใช้ List ของนักศึกษาจากการทดลอง 2.2 เพิ่มเติม:

1. เขียน Function `findTopStudentByFaculty(List students, String faculty)` ที่คืนชื่อนักศึกษาที่ GPA สูงสุดในคณะที่ระบุ
2. เขียน Function `groupByFaculty(List students)` ที่คืน `Map<String, List>` โดยจัดกลุ่มนักศึกษาตามคณะ
3. ใช้ `sort()` เรียงนักศึกษาตาม GPA จากสูงไปต่ำ แล้วพิมพ์ข้อมูลนักศึกษาที่มี GPA สูงสุด 3 อันดับแรก

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
String findTopStudentByFaculty(List<Map<String, dynamic>> students, String faculty) {
  var facultyStudents = students.where((s) => s["faculty"] ==  faculty).toList()
;
   if (facultyStudents.isEmpty) {
    return "ไม่พบนักศึกษาในคณะ $faculty";
  }
  var topStudent = facultyStudents.reduce((a, b) {
    return (a["gpa"] as double) > (b["gpa"] as double) ? a : b;
    });
    
  return topStudent["name"];
}

Map<String, List<Map<String, dynamic>>> groupByFaculty(List<Map<String, dynamic>> students) {
  Map<String, List<Map<String, dynamic>>> grouped = {};

  for (var student in students) {
    String faculty = student["faculty"];
    
    // ถ้ายังไม่มี key คณะนี้ใน Map ให้สร้าง List มารองรับก่อน
    grouped.putIfAbsent(faculty, () => []);
    
    // เพิ่มข้อมูลนักศึกษาเข้าไปในคณะนั้นๆ
    grouped[faculty]!.add(student);
  }

  return grouped;
}

void main() {
  List<Map<String, dynamic>> students = [
    {"name": "สมชาย",  "gpa": 3.75, "year": 3, "faculty": "วิศวกรรม"},
    {"name": "สมหญิง", "gpa": 2.50, "year": 1, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศักดิ์","gpa": 3.10, "year": 2, "faculty": "วิศวกรรม"},
    {"name": "สมใจ",  "gpa": 1.80, "year": 4, "faculty": "บริหาร"},
    {"name": "สมปอง", "gpa": 3.50, "year": 2, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศรี", "gpa": 2.90, "year": 3, "faculty": "บริหาร"},
  ];

  // === where() — กรองนักศึกษาที่ GPA >= 3.0 ===
  print("=== นักศึกษาที่ GPA >= 3.0 ===");
  var honorStudents = students
      .where((s) => (s["gpa"] as double) >= 3.0)
      .toList();
  for (var s in honorStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }

  // === map() — แปลงเป็น String รายงาน ===
  print("\n=== รายงานนักศึกษา ===");
  var report = students
      .map((s) => "${s["name"]} (${s["faculty"]}) GPA: ${s["gpa"]}")
      .toList();
  report.forEach(print);

  // === sort() + reduce() ===
  print("\n=== วิเคราะห์คะแนน ===");
  List<double> gpas = students.map((s) => s["gpa"] as double).toList();

  double maxGpa = gpas.reduce((a, b) => a > b ? a : b);
  double minGpa = gpas.reduce((a, b) => a < b ? a : b);
  double avgGpa = gpas.reduce((a, b) => a + b) / gpas.length;

  print("GPA สูงสุด: $maxGpa");
  print("GPA ต่ำสุด: $minGpa");
  print("GPA เฉลี่ย: ${avgGpa.toStringAsFixed(2)}");

  // === any() และ every() ===
  bool anyFailing = students.any((s) => (s["gpa"] as double) < 2.0);
  bool allPassing = students.every((s) => (s["gpa"] as double) >= 2.0);
  print("มีนักศึกษาที่ GPA < 2.0: $anyFailing");
  print("ทุกคน GPA >= 2.0: $allPassing");
  
    // ทดลองเพิ่มเอง: กรองเฉพาะคณะวิศวกรรม
  print("\n=== นักศึกษาคณะวิศวกรรม ===");
  var engineeringStudents = students
      .where((s) => s["faculty"] == "วิศวกรรม")
      .toList();
  // พิมพ์ชื่อและ GPA ของนักศึกษาแต่ละคน
  for (var s in engineeringStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }
  print("\n=== นักศึกษาที่ได้ GPA สูงสุดรายคณะ ===");
  String topEngineering = findTopStudentByFaculty(students, "วิศวกรรม");
  String topScience = findTopStudentByFaculty(students, "วิทยาศาสตร์");
  String topBusiness = findTopStudentByFaculty(students, "บริหาร");

  print("Top คณะวิศวกรรม: $topEngineering");
  print("Top คณะวิทยาศาสตร์: $topScience");
  print("Top คณะบริหาร: $topBusiness");
  
  // === ทดสอบ groupByFaculty ===
  print("\n=== จัดกลุ่มนักศึกษาตามคณะ ===");
  var groupedData = groupByFaculty(students);
  
  groupedData.forEach((faculty, list) {
    print("คณะ $faculty:");
    for (var s in list) {
      print("  - ${s["name"]} (GPA: ${s["gpa"]})");
    }
  });
  
  // === sort() — เรียงลำดับ GPA จากสูงไปต่ำ และหา Top 3 ===
  print("\n=== นักศึกษาที่มี GPA สูงสุด 3 อันดับแรก ===");
  
  // โคลน List ออกมาก่อนเพื่อไม่ให้กระทบกับข้อมูลต้นฉบับ
  var sortedStudents = students.toList();
  
  // sort จากสูงไปต่ำ: ถ้า b มากกว่า a ให้สลับตำแหน่งกัน
  sortedStudents.sort((a, b) => (b["gpa"] as double).compareTo(a["gpa"] as double));

  // ใช้ take(3) เพื่อเลือกเอาเฉพาะ 3 ตัวแรก
  var topThree = sortedStudents.take(3);

  // วนลูปแสดงผลพร้อมลำดับ
  int rank = 1;
  for (var s in topThree) {
    print("อันดับที่ $rank: ${s["name"]} (คณะ: ${s["faculty"]}) - GPA: ${s["gpa"]}");
    rank++;
  }
}
```
**Screenshot**
  <img width="1470" height="921" alt="image" src="https://github.com/user-attachments/assets/187bef68-c31d-479c-b985-ac2d343a96b4" />

---

## ส่วนที่ 3 — ทฤษฎีและการทดลอง: OOP

### ทฤษฎี 3.1 — Class และ Object

**Class** คือ "แบบพิมพ์" หรือ "Blueprint" ของ Object ส่วน **Object** คือ "สิ่งของ" ที่สร้างจาก Class นั้น

```
Class (แบบพิมพ์):          Object (สิ่งของ):
┌───────────────────┐      ┌───────────────────┐
│  class Student {  │  →   │  student1         │
│    String name;   │      │    name: "สมชาย"  │
│    int age;       │      │    age: 20        │
│    study() {...}  │      │    study() → ทำงาน│
│  }                │      └───────────────────┘
└───────────────────┘
                           ┌───────────────────┐
                       →   │  student2         │
                           │    name: "สมหญิง"  │
                           │    age: 21        │
                           └───────────────────┘
```

**โครงสร้าง Class ใน Dart:**

```dart
class BankAccount {
  // === Fields (ตัวแปรของ Object) ===
  final String accountNumber;   // final = เปลี่ยนหลัง Constructor ไม่ได้
  final String ownerName;
  double _balance;               // _ นำหน้า = private (ใช้ได้ใน Class นี้เท่านั้น)
  List<String> _transactions = [];

  // === Constructor (สร้าง Object) ===
  // this.xxx = กำหนดค่า field โดยตรง ไม่ต้องเขียน body
  BankAccount({
    required this.accountNumber,
    required this.ownerName,
    double initialBalance = 0,
  }) : _balance = initialBalance; // initializer list

  // Named Constructor — สร้าง Object แบบพิเศษ
  BankAccount.savings({required String owner})
      : accountNumber = "SAV${DateTime.now().millisecondsSinceEpoch}",
        ownerName = owner,
        _balance = 0;

  // === Getter — อ่านค่าแบบ Property ===
  double get balance => _balance;
  bool get isEmpty => _balance == 0;
  List<String> get transactions => List.unmodifiable(_transactions);

  // === Methods ===
  bool deposit(double amount) {
    if (amount <= 0) return false;
    _balance += amount;
    _transactions.add("ฝาก: +${amount.toStringAsFixed(2)}");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0 || amount > _balance) return false;
    _balance -= amount;
    _transactions.add("ถอน: -${amount.toStringAsFixed(2)}");
    return true;
  }

  // toString — แสดงเมื่อ print(object)
  @override
  String toString() =>
      "บัญชี[$accountNumber] ของ $ownerName ยอด: ${_balance.toStringAsFixed(2)}";
}
```

---

### ทฤษฎี 3.2 — Inheritance (การสืบทอด) และ Abstract Class

**Inheritance** คือการสร้าง Class ใหม่จาก Class เดิม โดยสืบทอดทุกอย่างมา แล้วเพิ่มหรือแก้ไขเฉพาะส่วนที่ต่าง

**Abstract Class** คือ Class ที่กำหนด "สัญญา" ว่า Subclass ต้อง implement อะไรบ้าง ไม่สามารถสร้าง Object โดยตรงได้

```
Abstract Class (สัญญา):
┌───────────────────────────────────────────┐
│  abstract class Shape {                   │
│    double get area;       ← ต้อง implement │
│    double get perimeter;  ← ต้อง implement │
│    void describe() {...}  ← มาให้แล้ว       │
│  }                                        │
└───────────────────────────────────────────┘
              ↑ extends
   ┌──────────┴──────────┐
   │                     │
Circle               Rectangle
┌──────────┐       ┌──────────────┐
│ radius   │       │ width,height │
│ area=πr² │       │ area=w*h     │
│ perim=2πr│       │ perim=2(w+h) │
└──────────┘       └──────────────┘
```

```dart
abstract class Shape {
  // Abstract getter — Subclass ต้อง implement
  double get area;
  double get perimeter;

  // Concrete method — มาให้แล้ว Subclass ใช้ได้เลย
  void describe() {
    print("${runtimeType}:");
    print("  พื้นที่: ${area.toStringAsFixed(2)} ตร.หน่วย");
    print("  เส้นรอบรูป: ${perimeter.toStringAsFixed(2)} หน่วย");
  }

  bool isLargerThan(Shape other) => area > other.area;
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);

  @override
  double get area => 3.14159 * radius * radius;

  @override
  double get perimeter => 2 * 3.14159 * radius;
}

class Rectangle extends Shape {
  final double width;
  final double height;
  Rectangle(this.width, this.height);

  @override
  double get area => width * height;

  @override
  double get perimeter => 2 * (width + height);

  // Subclass สามารถเพิ่ม method เองได้
  bool get isSquare => width == height;
}
```

---

### ทฤษฎี 3.3 — Mixin

**Mixin** คือชุด Method ที่เพิ่มให้กับ Class ได้ โดยไม่ต้อง Inherit (เหมาะเมื่อต้องการเพิ่ม Behavior หลายชุดให้กับ Class เดียว)

```
ปัญหา: Dart ให้ extends ได้แค่ 1 Class
แต่บางครั้งต้องการ Behavior จากหลายที่

Mixin แก้ปัญหาโดย:
class Duck extends Animal with Swimmable, Flyable {
  // Duck ได้ทั้ง swim() และ fly() โดยไม่ต้อง inherit สองชั้น
}
```

```dart
mixin Printable {
  // Mixin สามารถมี Method และ Getter
  void printInfo() {
    print(toString()); // เรียก toString() ของ Class ที่ใช้ Mixin
  }
}

mixin Saveable {
  Map<String, dynamic> toJson(); // บังคับให้ Class ที่ใช้ implement

  String toJsonString() {
    var json = toJson();
    return json.entries.map((e) => '"${e.key}": "${e.value}"').join(", ");
  }
}

// ใช้ Mixin หลายตัวพร้อมกัน
class Product with Printable, Saveable {
  final String name;
  final double price;
  final int stock;

  Product({required this.name, required this.price, required this.stock});

  @override
  Map<String, dynamic> toJson() => {
    "name": name,
    "price": price,
    "stock": stock,
  };

  @override
  String toString() => "$name (฿${price.toStringAsFixed(2)}) เหลือ $stock ชิ้น";
}
```

---

### การทดลอง 3.1 — Class และ Inheritance

**⏱ เวลา:** 30 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ด BankAccount:

```dart
class BankAccount {
  final String ownerName;
  double _balance;
  List<String> _history = [];

  BankAccount({required this.ownerName, double initial = 0})
      : _balance = initial;

  double get balance => _balance;
  List<String> get history => List.unmodifiable(_history);

  bool deposit(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    _balance += amount;
    _history.add("+ ฝาก ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ฝาก ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    if (amount > _balance) {
      print("❌ ยอดเงินไม่เพียงพอ (มี ${_balance.toStringAsFixed(2)} บาท)");
      return false;
    }
    _balance -= amount;
    _history.add("- ถอน ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  void printStatement() {
    print("\n=== สรุปบัญชี: $ownerName ===");
    print("ยอดปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท");
    print("ประวัติรายการ:");
    if (_history.isEmpty) {
      print("  (ยังไม่มีรายการ)");
    } else {
      _history.forEach((h) => print("  $h"));
    }
  }

  @override
  String toString() => "BankAccount(${ownerName}, ยอด: ${_balance.toStringAsFixed(2)})";
}
```

**ขั้นตอนที่ 2** เพิ่ม Subclass SavingsAccount ที่มีดอกเบี้ย:

```dart
class SavingsAccount extends BankAccount {
  final double interestRate; // อัตราดอกเบี้ยต่อปี เช่น 0.03 = 3%

  SavingsAccount({
    required String ownerName,
    required this.interestRate,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  // Override withdraw เพื่อเพิ่มกฎพิเศษ
  @override
  bool withdraw(double amount) {
    if (_balance - amount < 500) {
      print("❌ บัญชีออมทรัพย์ต้องมียอดขั้นต่ำ 500 บาท");
      return false;
    }
    return super.withdraw(amount); // เรียก withdraw() ของ BankAccount
  }

  // Method พิเศษของ SavingsAccount
  void applyMonthlyInterest() {
    double interest = _balance * interestRate / 12;
    _balance += interest;
    _history.add("+ ดอกเบี้ยรายเดือน ${interest.toStringAsFixed(2)} บาท");
    print("✅ ดอกเบี้ยเดือนนี้: ${interest.toStringAsFixed(2)} บาท");
  }
}
```

**ขั้นตอนที่ 3** เพิ่ม `main()` และรัน:

```dart
void main() {
  print("=== ทดสอบ BankAccount ===\n");
  var acc = BankAccount(ownerName: "สมชาย", initial: 1000);

  acc.deposit(500);
  acc.withdraw(200);
  acc.withdraw(2000); // เกินยอด
  acc.withdraw(-100); // ค่าไม่ถูก
  acc.printStatement();

  print("\n=== ทดสอบ SavingsAccount ===\n");
  var savings = SavingsAccount(
    ownerName: "สมหญิง",
    interestRate: 0.03,
    initial: 1000,
  );

  savings.deposit(5000);
  savings.withdraw(5600); // เหลือน้อยกว่า 500
  savings.withdraw(3000); // ได้
  savings.applyMonthlyInterest();
  savings.printStatement();

  // Polymorphism — ใช้ BankAccount แทนทั้งคู่ได้
  print("\n=== Polymorphism ===");
  List<BankAccount> accounts = [acc, savings];
  for (var account in accounts) {
    print(account); // เรียก toString() ของแต่ละ Object
  }
}
```

**ขั้นตอนที่ 4** กด Run และอ่านผลลัพธ์ทุกบรรทัด
- Screenshot
  <img width="1470" height="923" alt="image" src="https://github.com/user-attachments/assets/f22ac595-7e1b-4536-a9c2-3bcff62aa456" />

---

### 🎯 โจทย์ฝึกทำ 3 — เขียน Class ด้วยตนเอง

1. สร้าง `CheckingAccount extends BankAccount` ที่อนุญาตให้ถอนเกินยอดได้ไม่เกิน 500 บาท (Overdraft) และคิดค่าธรรมเนียม 50 บาท เมื่อ Overdraft

2. สร้าง Abstract Class `Vehicle` ที่มี Abstract getter `fuelEfficiency` (กม./ลิตร), method `refuel(double liters)`, method `drive(double km)` ที่คำนวณการใช้น้ำมัน จากนั้นสร้าง `Car` และ `Truck` ที่ extend `Vehicle` โดยมี `fuelEfficiency` ต่างกัน

3. สร้าง Mixin `Discountable` ที่มี method `applyDiscount(double percent)` แล้วนำไปใช้กับ Class `Product` ที่มี `name` และ `price`


**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
class BankAccount {
  final String ownerName;
  double _balance;
  List<String> _history = [];

  BankAccount({required this.ownerName, double initial = 0})
      : _balance = initial;

  double get balance => _balance;
  List<String> get history => List.unmodifiable(_history);

  bool deposit(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    _balance += amount;
    _history.add("+ ฝาก ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ฝาก ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    if (amount > _balance) {
      print("❌ ยอดเงินไม่เพียงพอ (มี ${_balance.toStringAsFixed(2)} บาท)");
      return false;
    }
    _balance -= amount;
    _history.add("- ถอน ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  void printStatement() {
    print("\n=== สรุปบัญชี: $ownerName ===");
    print("ยอดปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท");
    print("ประวัติรายการ:");
    if (_history.isEmpty) {
      print("  (ยังไม่มีรายการ)");
    } else {
      _history.forEach((h) => print("  $h"));
    }
  }

  @override
  String toString() => "BankAccount(${ownerName}, ยอด: ${_balance.toStringAsFixed(2)})";
}

class SavingsAccount extends BankAccount {
  final double interestRate; // อัตราดอกเบี้ยต่อปี เช่น 0.03 = 3%

  SavingsAccount({
    required String ownerName,
    required this.interestRate,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  // Override withdraw เพื่อเพิ่มกฎพิเศษ
  @override
  bool withdraw(double amount) {
    if (_balance - amount < 500) {
      print("❌ บัญชีออมทรัพย์ต้องมียอดขั้นต่ำ 500 บาท");
      return false;
    }
    return super.withdraw(amount); // เรียก withdraw() ของ BankAccount
  }

  // Method พิเศษของ SavingsAccount
  void applyMonthlyInterest() {
    double interest = _balance * interestRate / 12;
    _balance += interest;
    _history.add("+ ดอกเบี้ยรายเดือน ${interest.toStringAsFixed(2)} บาท");
    print("✅ ดอกเบี้ยเดือนนี้: ${interest.toStringAsFixed(2)} บาท");
  }
}

class CheckingAccount extends BankAccount {
  // กำหนดค่าวงเงินถอนเกินบัญชีสูงสุดเป็นค่าคงที่
  static const double maxOverdraft = 500.0;
  static const double overdraftFee = 50.0;

  CheckingAccount({
    required String ownerName,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  @override
  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }

    // กรณีที่ 1: ยอดเงินพอถอนปกติ (เรียกใช้ความสามารถของคลาสแม่ได้เลย)
    if (amount <= _balance) {
      return super.withdraw(amount);
    }

    // กรณีที่ 2: ถอนเกินยอดเงินที่มี (Overdraft)
    double shortage = amount - _balance; // จำนวนเงินที่ขาด
    double totalRequired = amount + overdraftFee; // จำนวนเงินที่จะถอนรวมกับค่าธรรมเนียม

    // ตรวจสอบว่าจำนวนเงินที่ขาด เกินวงเงิน Overdraft (500 บาท) หรือไม่
    // หรือถ้ายอดเงินที่มีไม่พอจ่ายค่าธรรมเนียมด้วย ก็จะถอนไม่ได้
    if (shortage > maxOverdraft) {
      print("❌ เกินวงเงิน Overdraft (สามารถถอนเกินได้ไม่เกิน ${maxOverdraft.toStringAsFixed(2)} บาท)");
      return false;
    }
    
    if (_balance - totalRequired < -maxOverdraft) {
      print("❌ ยอดเงินไม่เพียงพอสำหรับการหักรวมค่าธรรมเนียม Overdraft");
      return false;
    }

    // ทำการหักเงิน (ยอดติดลบได้) และหักค่าธรรมเนียม
    _balance -= totalRequired;
    
    // บันทึกประวัติ
    _history.add("- ถอน Overdraft ${amount.toStringAsFixed(2)} บาท (ค่าธรรมเนียม ${overdraftFee.toStringAsFixed(2)} บาท, ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("⚠️ Overdraft! ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ (คิดค่าธรรมเนียม ${overdraftFee.toStringAsFixed(2)} บาท)");
    
    return true;
  }
}

abstract class Vehicle {
  String brand;
  double _fuelAmount; // ปริมาณน้ำมันปัจจุบันในถัง (ลิตร)

  Vehicle({required this.brand, double initialFuel = 0}) : _fuelAmount = initialFuel;

  // 1. Abstract getter ที่คลาสลูกทุกคลาส "ต้อง" ไปกำหนดค่าเอง
  double get fuelEfficiency; 

  double get fuelAmount => _fuelAmount;

  // 2. Method เติมน้ำมัน (แชร์คำสั่งร่วมกันได้เลย)
  void refuel(double liters) {
    if (liters <= 0) {
      print("❌ จำนวนน้ำมันที่เติมต้องมากกว่า 0 ลิตร");
      return;
    }
    _fuelAmount += liters;
    print("⛽ [$brand] เติมน้ำมัน +${liters.toStringAsFixed(1)} ลิตร (น้ำมันคงเหลือ: ${_fuelAmount.toStringAsFixed(1)} ลิตร)");
  }

  // 3. Method ขับเคลื่อน (แชร์คำสั่งร่วมกัน แต่ดึงคำนวณจาก fuelEfficiency ของคลาสลูก)
  void drive(double km) {
    if (km <= 0) {
      print("❌ ระยะทางต้องมากกว่า 0 กม.");
      return;
    }

    // คำนวณน้ำมันที่ต้องใช้ = ระยะทาง / อัตราประหยัดน้ำมัน
    double fuelNeeded = km / fuelEfficiency;

    if (fuelNeeded > _fuelAmount) {
      print("❌ [$brand] น้ำมันไม่พอวิ่งได้ $km กม. (ต้องการ ${fuelNeeded.toStringAsFixed(1)} ลิตร แต่มีแค่ ${_fuelAmount.toStringAsFixed(1)} ลิตร)");
    } else {
      _fuelAmount -= fuelNeeded;
      print("🚗 [$brand] วิ่งไป $km กม. (ใช้น้ำมันไป ${fuelNeeded.toStringAsFixed(1)} ลิตร, คงเหลือ: ${_fuelAmount.toStringAsFixed(1)} ลิตร)");
    }
  }
}

class Car extends Vehicle {
  Car({required String brand, double initialFuel = 0}) 
      : super(brand: brand, initialFuel: initialFuel);

  // สมมุติว่ารถยนต์ทั่วไปวิ่งได้ 15 กิโลเมตร ต่อ น้ำมัน 1 ลิตร
  @override
  double get fuelEfficiency => 15.0; 
}

class Truck extends Vehicle {
  Truck({required String brand, double initialFuel = 0}) 
      : super(brand: brand, initialFuel: initialFuel);

  // รถบรรทุกคันใหญ่ กินน้ำมันมากกว่า วิ่งได้แค่ 6 กิโลเมตร ต่อ น้ำมัน 1 ลิตร
  @override
  double get fuelEfficiency => 6.0; 
}

// 1. สร้าง mixin สำหรับความสามารถในการลดราคา
mixin Discountable {
  // สร้าง method สำหรับคิดส่วนลดตามเปอร์เซ็นต์ที่ส่งเข้ามา
  double calculateDiscount(double currentPrice, double percent) {
    if (percent < 0 || percent > 100) {
      print("❌ เปอร์เซ็นต์ส่วนลดต้องอยู่ระหว่าง 0 ถึง 100");
      return 0.0;
    }
    return currentPrice * (percent / 100);
  }
}

// 2. สร้างคลาส Product และนำ mixin มาใช้ด้วยคีย์เวิร์ด `with`
class Product with Discountable {
  String name;
  double price;

  Product({required this.name, required this.price});

  // Method สำหรับการเรียกใช้ความสามารถจาก mixin เพื่อลดราคาสินค้าจริง
  void applyDiscount(double percent) {
    double discountAmount = calculateDiscount(price, percent);
    
    if (discountAmount > 0) {
      double oldPrice = price;
      price -= discountAmount; // ปรับลดราคาสินค้าลง
      print("🏷️ โค้ดส่วนลด $percent% สำหรับ $name สำเร็จ!");
      print("   ราคาเดิม: ${oldPrice.toStringAsFixed(2)} บาท -> ราคาใหม่: ${price.toStringAsFixed(2)} บาท");
    }
  }

  @override
  String toString() => "สินค้า: $name, ราคา: ${price.toStringAsFixed(2)} บาท";
}

void main() {
  print("=== ทดสอบ BankAccount ===\n");
  var acc = BankAccount(ownerName: "สมชาย", initial: 1000);

  acc.deposit(500);
  acc.withdraw(200);
  acc.withdraw(2000); // เกินยอด
  acc.withdraw(-100); // ค่าไม่ถูก
  acc.printStatement();

  print("\n=== ทดสอบ SavingsAccount ===\n");
  var savings = SavingsAccount(
    ownerName: "สมหญิง",
    interestRate: 0.03,
    initial: 1000,
  );
  

  print("\n=== ทดสอบ CheckingAccount ===");
  var checking = CheckingAccount(ownerName: "สมชาย (กระแสรายวัน)", initial: 1000);

  savings.deposit(5000);
  savings.withdraw(5600); // เหลือน้อยกว่า 500
  savings.withdraw(3000); // ได้
  savings.applyMonthlyInterest();
  savings.printStatement();

  // Polymorphism — ใช้ BankAccount แทนทั้งคู่ได้
  print("\n=== Polymorphism ===");
  List<BankAccount> accounts = [acc, savings];
  for (var account in accounts) {
    print(account); // เรียก toString() ของแต่ละ Object
  }

  print("\n=== ทดสอบระบบยานพาหนะ (Vehicle) ===\n");

  // สร้าง Object ของ Car และ Truck
  var myCar = Car(brand: "Toyota Civic", initialFuel: 10); // มีน้ำมันเริ่มแรก 10 ลิตร
  var myTruck = Truck(brand: "Isuzu Elf", initialFuel: 20); // มีน้ำมันเริ่มแรก 20 ลิตร

  // --- ทดสอบรถยนต์ (Car) ---
  print("--- [Car Testing] ---");
  myCar.drive(90);  // วิ่ง 90 กม. ใช้ 90/15 = 6 ลิตร (เหลือ 4 ลิตร)
  myCar.drive(100); // วิ่งอีก 100 กม. ใช้ 100/15 = 6.6 ลิตร (น้ำมันไม่พอ!)
  myCar.refuel(20); // เติมเงินเพิ่ม 20 ลิตร
  myCar.drive(100); // วิ่งใหม่รอบนี้ผ่านฉลุย
  
  print("");

  // --- ทดสอบรถบรรทุก (Truck) ---
  print("--- [Truck Testing] ---");
  myTruck.drive(90);  // วิ่ง 90 กม. ใช้ 90/6 = 15 ลิตร (เหลือ 5 ลิตร)
  myTruck.drive(60);  // วิ่งอีก 60 กม. ใช้ 60/6 = 10 ลิตร (น้ำมันไม่พอ!)
  myTruck.refuel(30); // เติมน้ำมันเพิ่ม 30 ลิตร
  myTruck.drive(60);  // วิ่งได้แล้ว
  
  print("\n=== ทดสอบระบบสินค้าและส่วนลด (Mixin) ===");
  
  // สร้างสินค้าชิ้นที่ 1
  var laptop = Product(name: "Gaming Laptop", price: 35000.0);
  print(laptop);
  laptop.applyDiscount(10); // ลดราคา 10%
  print(laptop);

  print("");

  // สร้างสินค้าชิ้นที่ 2
  var shoes = Product(name: "Running Shoes", price: 2500.0);
  print(shoes);
  shoes.applyDiscount(20); // ลดราคา 20%
  print(shoes);

  // ทดสอบใส่ค่าที่ผิดพลาด
  shoes.applyDiscount(150); // เกิน 100% (จะแสดงข้อความแจ้งเตือน)

}
```
**Screenshot**
  <img width="1470" height="920" alt="image" src="https://github.com/user-attachments/assets/b86c1225-8c17-4429-bada-9ec79d374105" />

---

## ส่วนที่ 4 — ทฤษฎีและการทดลอง: Async/Await และ Future

### ทฤษฎี 4.1 — ทำไม Mobile App ถึงต้องมี Async?

ลองนึกถึงแอปสั่งอาหาร เมื่อผู้ใช้กด "สั่งอาหาร" แอปต้องส่งข้อมูลไปยัง Server แล้วรอการยืนยัน ซึ่งอาจใช้เวลา 1-3 วินาที

```
ถ้าใช้ Synchronous (รอแบบบล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด UI][รอ Server 3 วินาที.........][วาด UI]
                          ↑
             ผู้ใช้กดอะไรก็ไม่ตอบสนอง
             ระบบแจ้ง "App Not Responding"

ถ้าใช้ Asynchronous (รอแบบไม่บล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด][แสดง Loading][วาด][วาด][แสดงผล]
Background:  [ส่งไป Server──────────►รับผล]
```

**Future คืออะไร:**

```
Future<T> = "คำสัญญาว่าจะได้ T ในอนาคต"

Future<String>  → จะได้ String ในภายหลัง
Future<int>     → จะได้ int ในภายหลัง
Future<void>    → จะเสร็จในภายหลัง (ไม่มีค่าคืน)

สถานะของ Future:
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│  Pending    │ →  │  Completed   │ or │   Error     │
│  (รอผล)     │    │  (มีผลแล้ว)    │    │  (เกิดข้อ     │
│             │    │              │    │   ผิดพลาด)   │
└─────────────┘    └──────────────┘    └─────────────┘
```

---

### ทฤษฎี 4.2 — async/await และ Error Handling

```dart
// ประกาศ async function
Future<String> fetchUserName(int userId) async {
  // await หยุดรอ Future โดยไม่บล็อก Thread หลัก
  await Future.delayed(Duration(seconds: 1)); // จำลองการรอ Network

  if (userId <= 0) {
    // throw ส่ง Error ออกไป
    throw ArgumentError("userId ต้องมากกว่า 0");
  }
  return "ผู้ใช้ #$userId";
}

// เรียกใช้ด้วย await
void main() async {
  // try/catch จัดการ Error
  try {
    String name = await fetchUserName(1);
    print("ได้รับ: $name");

    // จะ throw ArgumentError
    await fetchUserName(-1);

  } on ArgumentError catch (e) {
    // จับ Error เฉพาะประเภท
    print("Argument Error: $e");
  } catch (e, stackTrace) {
    // จับ Error ทุกประเภท
    print("Error: $e");
    print("Stack: $stackTrace");
  } finally {
    // รันเสมอ ไม่ว่าจะ Error หรือไม่
    print("เสร็จสิ้น");
  }
}
```

**Sequential vs Parallel Execution:**

```dart
// Sequential — รอทีละอย่าง (เสียเวลา)
Future<void> loadSequential() async {
  var user    = await fetchUser(1);      // รอ 1 วินาที
  var posts   = await fetchPosts(1);     // รออีก 0.8 วินาที
  var friends = await fetchFriends(1);   // รออีก 0.5 วินาที
  // รวม ~2.3 วินาที
}

// Parallel — รอพร้อมกัน (เร็วกว่า)
Future<void> loadParallel() async {
  var results = await Future.wait([
    fetchUser(1),       // ╗
    fetchPosts(1),      // ╠═ รันพร้อมกันทั้งหมด
    fetchFriends(1),    // ╝
  ]);
  // รวม ~1 วินาที (เท่ากับ task ที่นานสุด)
  var user    = results[0];
  var posts   = results[1];
  var friends = results[2];
}
```

**Stream — ข้อมูลที่ไหลต่อเนื่อง:**

```dart
// Stream คือ "ท่อ" ที่ส่งข้อมูลหลายๆ ครั้งตามเวลา
Stream<int> countDown(int from) async* {
  for (int i = from; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // ส่งค่าออกทาง Stream
  }
}

// รับข้อมูลจาก Stream
void main() async {
  await for (int count in countDown(5)) {
    print("นับถอยหลัง: $count");
  }
  print("🚀 ปล่อย!");
}
```

---

### การทดลอง 4.1 — Future และ async/await

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดจำลอง API:

```dart
import 'dart:async';

// จำลอง Database/API functions
Future<Map<String, dynamic>> fetchUser(int id) async {
  print("  [API] กำลังดึงข้อมูล User $id...");
  await Future.delayed(Duration(milliseconds: 800)); // จำลอง Network delay

  if (id <= 0) throw Exception("User ID ไม่ถูกต้อง");

  return {
    "id": id,
    "name": "ผู้ใช้ที่ $id",
    "email": "user$id@example.com",
    "role": id == 1 ? "admin" : "user",
  };
}

Future<List<String>> fetchUserPosts(int userId) async {
  print("  [API] กำลังดึง Posts ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 600));

  return [
    "โพสต์ที่ 1 ของ User $userId",
    "โพสต์ที่ 2 ของ User $userId",
    "โพสต์ที่ 3 ของ User $userId",
  ];
}

Future<int> fetchUserFollowers(int userId) async {
  print("  [API] กำลังดึงจำนวน Follower ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 500));
  return userId * 42; // จำลอง
}
```

**ขั้นตอนที่ 2** เพิ่ม `main()` ทดสอบแบบ Sequential:

```dart
void main() async {
  print("=== Sequential (รอทีละอย่าง) ===");
  var stopwatch = Stopwatch()..start();

  var user      = await fetchUser(1);
  var posts     = await fetchUserPosts(1);
  var followers = await fetchUserFollowers(1);

  stopwatch.stop();
  print("ชื่อ: ${user['name']}");
  print("จำนวนโพสต์: ${posts.length}");
  print("Followers: $followers คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms\n");

  // === Parallel ===
  print("=== Parallel (Future.wait) ===");
  stopwatch = Stopwatch()..start();

  var results = await Future.wait([
    fetchUser(2),
    fetchUserPosts(2),
    fetchUserFollowers(2),
  ]);

  stopwatch.stop();
  var user2      = results[0] as Map<String, dynamic>;
  var posts2     = results[1] as List<String>;
  var followers2 = results[2] as int;

  print("ชื่อ: ${user2['name']}");
  print("จำนวนโพสต์: ${posts2.length}");
  print("Followers: $followers2 คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms");
}
```

**ขั้นตอนที่ 3** กด Run สังเกตความแตกต่างของเวลา
- Screenshot
  <img width="1470" height="924" alt="image" src="https://github.com/user-attachments/assets/7f96ef8b-b7b1-46b5-93e0-cc7260e2576d" />

**ขั้นตอนที่ 4** เพิ่ม Error Handling ต่อท้าย `main()`:

```dart
  // === Error Handling ===
  print("\n=== Error Handling ===");

  try {
    print("ลองดึง User ID = -1:");
    var badUser = await fetchUser(-1);
    print("ได้รับ: $badUser"); // ไม่ถึงบรรทัดนี้
  } on Exception catch (e) {
    print("❌ Exception: $e");
  }

  print("โปรแกรมยังทำงานต่อได้หลัง Error ✅");
```

**ขั้นตอนที่ 5** กด Run อีกครั้ง บันทึกผลเวลาของ Sequential vs Parallel
- Screenshot
  <img width="1470" height="923" alt="image" src="https://github.com/user-attachments/assets/139011d8-524f-4df6-9984-6b5adb1763bb" />

```
บันทึกผลการทดลอง:
Sequential ใช้เวลา: 1915 ms
Parallel ใช้เวลา:   802 ms
ประหยัดเวลาได้:     1113 ms (58.07 %)
```

---

### การทดลอง 4.2 — Stream

**⏱ เวลา:** 15 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์:

```dart
import 'dart:async';

// Stream Generator ด้วย async*
Stream<double> simulateStockPrice(String symbol) async* {
  double price = 100.0;
  int ticks = 0;

  while (ticks < 5) {
    await Future.delayed(Duration(milliseconds: 500));

    // จำลองการเปลี่ยนราคา
    double change = (ticks % 2 == 0) ? 2.5 : -1.5;
    price += change;
    ticks++;

    yield price; // ส่งค่าออกทาง Stream
  }
}

void main() async {
  print("=== ราคาหุ้น (Stream) ===");
  print("Symbol: DART\n");

  double? lastPrice;

  await for (double price in simulateStockPrice("DART")) {
    String direction = "";
    if (lastPrice != null) {
      direction = price > lastPrice! ? "📈 ขึ้น" : "📉 ลง";
    }
    print("ราคา: ${price.toStringAsFixed(2)} บาท  $direction");
    lastPrice = price;
  }

  print("\nสิ้นสุดการแสดงราคา");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตว่าราคาออกมาทีละค่า ไม่ใช่ทั้งหมดพร้อมกัน
- Screenshot
  <img width="1470" height="922" alt="image" src="https://github.com/user-attachments/assets/e3c032d3-39c2-4e9e-a7de-ad5d44d7ef4c" />

---

### 🎯 โจทย์ฝึกทำ 4 — เขียน Async ด้วยตนเอง

1. สร้าง `Future<double> calculateTax(double income)` ที่มี delay 0.5 วินาที คืนค่าภาษีตามอัตราก้าวหน้า (income <= 150,000 → 0%, <= 300,000 → 5%, <= 500,000 → 10%, อื่นๆ → 20%)

2. เขียน `main()` ที่ดึงข้อมูลรายได้ของผู้ใช้ 3 คนพร้อมกัน (ใช้ `Future.wait`) แล้วคำนวณภาษีแต่ละคน และแสดงผลรวมภาษีทั้งหมด

3. สร้าง `Stream<String>` ที่จำลองการส่ง Chat Message ทุก 1 วินาที เป็นเวลา 5 ครั้ง แล้วแสดงผลผ่าน `await for`

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
import 'dart:async';

// 1. ฟังก์ชันคำนวณภาษี (Future)
Future<double> calculateTax(double income) async {
  await Future.delayed(Duration(milliseconds: 500)); 

  if (income <= 150000) {
    return 0.0;
  } else if (income <= 300000) {
    return (income - 150000) * 0.05;
  } else if (income <= 500000) {
    return (150000 * 0.05) + ((income - 300000) * 0.10);
  } else {
    return (150000 * 0.05) + (200000 * 0.10) + ((income - 500000) * 0.20);
  }
}

// 2. ฟังก์ชันจำลองราคาหุ้น (Stream 1)
Stream<double> simulateStockPrice(String symbol) async* {
  double price = 100.0;
  int ticks = 0;

  while (ticks < 5) {
    await Future.delayed(Duration(milliseconds: 500));
    double change = (ticks % 2 == 0) ? 2.5 : -1.5;
    price += change;
    ticks++;
    yield price; 
  }
}

// === [ส่วนที่เพิ่มใหม่] ฟังก์ชันจำลอง Chat Message ทุก 1 วินาที 5 ครั้ง (Stream 2) ===
Stream<String> simulateChatMessage() async* {
  List<String> messages = [
    "สวัสดีครับ ยินดีที่ได้รู้จัก",
    "กำลังศึกษาเรื่อง Stream ใน Dart อยู่เหรอครับ?",
    "มันมีประโยชน์มาก ๆ เลยนะสำหรับการเขียนแอพพลิเคชัน",
    "ข้อความที่ 4 กำลังจะมาแล้ว...",
    "เย้! นี่คือข้อความสุดท้าย บ๊ายบายครับ 👋"
  ];

  for (int i = 0; i < messages.length; i++) {
    await Future.delayed(Duration(seconds: 1)); // หน่วงเวลา 1 วินาทีตามโจทย์
    yield "[User]: ${messages[i]}"; // ส่งข้อความออกทาง Stream
  }
}

void main() async {
  // --- ส่วนที่ 1: ราคาหุ้น ---
  print("=== ราคาหุ้น (Stream) ===");
  print("Symbol: DART\n");

  double? lastPrice;
  await for (double price in simulateStockPrice("DART")) {
    String direction = "";
    if (lastPrice != null) {
      direction = price > lastPrice ? "📈 ขึ้น" : "📉 ลง";
    }
    print("ราคา: ${price.toStringAsFixed(2)} บาท  $direction");
    lastPrice = price;
  }
  print("\nสิ้นสุดการแสดงราคา");
  
  // --- ส่วนที่ 2: คำนวณภาษี ---
  print("\n-----------------------------------");
  print("=== คำนวณภาษีเงินได้ 3 คนพร้อมกัน (Future.wait) ===");
  
  double incomePerson1 = 250000;
  double incomePerson2 = 450000;
  double incomePerson3 = 750000;

  print("กำลังคำนวณภาษีของทั้ง 3 คนพร้อมกัน...");

  List<double> taxes = await Future.wait([
    calculateTax(incomePerson1),
    calculateTax(incomePerson2),
    calculateTax(incomePerson3),
  ]);

  double tax1 = taxes[0];
  double tax2 = taxes[1];
  double tax3 = taxes[2];

  print("คนที่ 1 (รายได้ ${incomePerson1.toStringAsFixed(0)}): ภาษีที่ต้องจ่าย = ${tax1.toStringAsFixed(2)} บาท");
  print("คนที่ 2 (รายได้ ${incomePerson2.toStringAsFixed(0)}): ภาษีที่ต้องจ่าย = ${tax2.toStringAsFixed(2)} บาท");
  print("คนที่ 3 (รายได้ ${incomePerson3.toStringAsFixed(0)}): ภาษีที่ต้องจ่าย = ${tax3.toStringAsFixed(2)} บาท");

  double totalTax = tax1 + tax2 + tax3;
  print("\n💰 ผลรวมภาษีทั้งหมดที่ต้องจ่าย: ${totalTax.toStringAsFixed(2)} บาท");

  // === [ส่วนที่เพิ่มใหม่] การดึงข้อความ Chat ด้วย await for ===
  print("\n-----------------------------------");
  print("=== จำลองระบบ Chat Message (Stream) ===");
  print("กำลังเชื่อมต่อห้องแชท...\n");

  // วนลูปเพื่อรอรับข้อความทีละข้อความเมื่อมันถูกปล่อยออกมา (yield) จาก Stream
  await for (String message in simulateChatMessage()) {
    print(message); 
  }

  print("\nห้องแชทปิดการเชื่อมต่อ");
}
```
**Screenshot**
  <img width="1470" height="921" alt="image" src="https://github.com/user-attachments/assets/f3230be8-19e4-44c8-82da-fc10fe146531" />

---


### คำถามท้ายใบงาน

#### **ข้อ 1** อธิบายความแตกต่างระหว่าง `final` และ `const` พร้อมยกตัวอย่างกรณีที่ใช้แต่ละแบบ
- const (Compile-time constant): ค่าของตัวแปรจะต้องถูกกำหนดและรู้ผลตั้งแต่ตอนที่คอมไพล์โค้ด (Compile-time) ค่านี้จะถูกฝังลงในหน่วยความจำและไม่สามารถเปลี่ยนได้อีกเลยตลอดกาล
- final (Runtime constant): ค่าของตัวแปรสามารถมารู้ผลตอนที่โปรแกรมกำลังทำงานอยู่ได้ (Runtime) แต่เมื่อถูกกำหนดค่าให้มันเป็นครั้งแรกแล้ว จะไม่สามารถแก้ไขหรือ Assign ค่าใหม่ให้มันได้อีกเลย

**ตัวอย่างสถานการณ์และการใช้งาน**
- กรณีที่ใช้ const: ค่าคงที่ทางคณิตศาสตร์, ค่ากำหนดระบบที่ไม่เปลี่ยนแน่นอน หรือ Widget ใน Flutter ที่ไม่มีวันเปลี่ยนแปลงค่า

```Dart
const double pi = 3.14159; // รู้ค่าแน่นอนตั้งแต่เขียนโค้ด
const Text('Hello World'); // Flutter Widget ที่ไม่ต้องวาดใหม่
```
กรณีที่ใช้ final: ค่าที่ได้จากการดึงข้อมูลจากฐานข้อมูล, เวลา ณ ปัจจุบัน หรือค่าที่รับมาจาก User

```Dart
final DateTime now = DateTime.now(); // ต้องรันโปรแกรมก่อนถึงจะรู้ว่าเวลากี่โมง
final String username = fetchUserData(); // ต้องรอผลลัพธ์จาก API
```
#### **ข้อ 2** Named Parameters และ Positional Parameters ต่างกันอย่างไร? ควรเลือกใช้แบบไหนเมื่อไหร่?
ทั้งสองแบบคือวิธีกำหนดค่า Parameter ให้กับฟังก์ชัน (หรือ Constructor) ตอนที่เรียกใช้งาน
- Positional Parameters (พารามิเตอร์ตามตำแหน่ง): การส่งค่าให้ฟังก์ชันเรียงตามลำดับที่ประกาศไว้ ห้ามสลับตำแหน่งเด็ดขาด

```Dart
void createUser(String name, int age) { ... }
// เวลาเรียกใช้: ต้องใส่ name ก่อน age เสมอ
createUser('Somchai', 25);
```
- Named Parameters (พารามิเตอร์ตามชื่อ): การส่งค่าโดยระบุชื่อพารามิเตอร์ผ่านวงเล็บปีกกา {} ไม่จำเป็นต้องเรียงลำดับ แต่ต้องระบุชื่อให้ถูกต้อง

```Dart
void createUser({required String name, int? age}) { ... }
// เวลาเรียกใช้: สลับตำแหน่งได้ และเข้าใจง่ายขึ้น
createUser(age: 25, name: 'Somchai');
```
**ควรเลือกใช้แบบไหนเมื่อไหร่?**
- เลือกใช้ Positional Parameters เมื่อ: * ฟังก์ชันมีพารามิเตอร์น้อย (1-2 ตัว) และความหมายชัดเจนในตัวเอง เช่น min(5, 10) หรือ print(message)
- เลือกใช้ Named Parameters เมื่อ:
  - ฟังก์ชันมีพารามิเตอร์หลายตัว (มากกว่า 2 ตัวขึ้นไป) เพราะจะช่วยป้องกันการส่งค่าสลับตำแหน่ง
  - พารามิเตอร์บางตัวเป็น Optional (ใส่หรือไม่ใส่ก็ได้)
  - ต้องการให้โค้ดอ่านง่ายขึ้นตอนเรียกใช้งาน (Readability) เช่น ใน Flutter Widget ส่วนใหญ่จะใช้ Named Parameters ทั้งหมด
#### **ข้อ 3** Abstract Class และ Mixin มีจุดประสงค์ต่างกันอย่างไร? ยกตัวอย่างสถานการณ์ที่เหมาะกับแต่ละแบบ
- Abstract Class (คลาสโครงร่าง): มีจุดประสงค์เพื่อทำ Subtyping (สร้างความสัมพันธ์แบบ Is-A / เป็นสิ่งนั้น) เพื่อเป็นพิมพ์เขียวหรือโครงสร้างพื้นฐานให้คลาสอื่นนำไปสืบทอด (Inherit/Extend) โดยคลาสลูกจะสามารถสืบทอดจากคลาสแม่ได้ เพียงคลาสเดียวเท่านั้น (Single Inheritance)
- Mixin (ส่วนเสริมพฤติกรรม): มีจุดประสงค์เพื่อทำ Code Reuse (สร้างความสัมพันธ์แบบ Has-A หรือ Can-Do / มีพฤติกรรมนั้น) เป็นการนิยามความสามารถหรือฟังก์ชันการทำงานที่คลาสอื่นๆ สามารถดึงไป "ผสมรวม (Mix-in)" ได้ โดยคลาสหนึ่งคลาสสามารถดึง Mixin ไปใช้ได้ หลายตัวพร้อมกัน (Multiple Reuse) โดยไม่ต้องมีความสัมพันธ์ทางสายเลือด (Hierarchy) เดียวกัน
**ตัวอย่างสถานการณ์ที่เหมาะ**
- สถานการณ์ของ Abstract Class:
  - โจทย์: คุณกำลังทำระบบจัดการสัตว์เลี้ยง คุณสร้าง Abstract Class ชื่อ Animal เพื่อบังคับให้สัตว์ทุกตัวต้องมีฟังก์ชัน makeSound() จากนั้นคลาส Dog และ Cat ก็มา extends Animal (หมา "คือ" สัตว์, แมว "คือ" สัตว์)
- สถานการณ์ของ Mixin:
  - โจทย์: คุณต้องการให้ทั้ง Dog (สัตว์) และ Airplane (เครื่องบิน) มีความสามารถในการ "ระบุพิกัด GPS ได้" แต่เครื่องบินไม่ใชสัตว์ และหมาไม่ใช่เครื่องบิน
  - วิธีแก้: สร้าง mixin GPSLocatable { void getCoordinates() { ... } } แล้วให้ทั้ง Dog และ Airplane นำไปใช้ผ่านคำสั่ง with GPSLocatable
#### **ข้อ 4** จากการทดลอง 4.1 Sequential ใช้เวลาประมาณกี่ ms และ Parallel ใช้เวลาเท่าไหร่? อธิบายเหตุผลที่ Parallel เร็วกว่า และบอกกรณีที่ต้องใช้ Sequential แทน
- Sequential (การทำงานแบบตามลำดับ): 1915 ms
- Parallel / Concurrent (การทำงานแบบคู่ขนาน/พร้อมกัน): 802 ms
- เหตุผลที่ Parallel เร็วกว่า
  - การทำงานแบบ Sequential คือการทำงานทีละอย่าง งานที่ 2 จะเริ่มได้ก็ต่อเมื่อ งานที่ 1 ทำเสร็จสิ้นแล้ว (เหมือนการต่อคิวซื้อของ) ทำให้เวลาทั้งหมดคือ ผลรวมของเวลาทุกงาน ในขณะที่   Parallel (ใน Dart มักหมายถึง Concurrency ผ่าน Future.wait) คือการปล่อยให้งานทุกงานเริ่มทำงานไปพร้อมๆ กันในเวลาเดียวกัน (เหมือนเปิดเคาน์เตอร์บริการพร้อมกันหลายช่อง) ทำให้เวลาที่ใช้ทั้งหมดจะเท่ากับ เวลาของงานที่ใช้เวลาทำนานที่สุดเพียงงานเดียว เท่านั้น

- กรณีที่จำเป็นต้องใช้ Sequential แทน
  เราจำเป็นต้องใช้ Sequential เมื่อ "งานถัดไป ต้องใช้ผลลัพธ์หรือข้อมูลจากงานก่อนหน้า" (Data Dependency)
  - ตัวอย่าง: คุณต้องทำระบบ Login โดยขั้นตอนคือ 1. ส่ง Username/Password ไปยืนยันตัวตนเพื่อเอา Token -> 2. นำ Token ที่ได้ไปดึงข้อมูล Profile ของ User
  - กรณีนี้คุณ ไม่สามารถ ทำแบบ Parallel ได้ เพราะขั้นตอนที่ 2 จะทำงานไม่ได้เลยหากขั้นตอนที่ 1 ยังไม่เสร็จสิ้นและไม่ได้ Token มา
    
#### **ข้อ 5** Future และ Stream ต่างกันอย่างไร? ยกตัวอย่างสถานการณ์ที่เหมาะกับแต่ละแบบจากการพัฒนา Mobile App จริงๆ
ทั้งสองตัวใช้จัดการกับการทำงานแบบไม่ประสานเวลา (Asynchronous Programming) แต่ต่างกันที่จำนวนของข้อมูลที่ส่งกลับมา

- Future (อนาคต): เปรียบเสมือน "กล่องรับของชิ้นเดียว" เป็นการทำงานที่จะส่งผลลัพธ์กลับมาเพียง ครั้งเดียวเท่านั้น (ไม่ว่าจะเป็นข้อมูลที่สำเร็จ หรือ Error ก็ตาม) แล้วก็จบการทำงานไป
- Stream (สายธารข้อมูล): เปรียบเสมือน "ท่อน้ำที่มีน้ำไหลมาเรื่อยๆ" เป็นการทำงานที่สามารถส่งผลลัพธ์กลับมาได้ หลายครั้ง/ต่อเนื่องเป็นชุดข้อมูล (Zero or more events) ตราบใดที่ท่อยยังไม่ถูกปิด

**ตัวอย่างสถานการณ์จริงในการพัฒนา Mobile App**
- สถานการณ์ที่เหมาะกับ Future:
  - การกดปุ่ม Login เพื่อตรวจสอบรหัสผ่าน (ส่งคำขอไป -> รอผลผ่าน/ไม่ผ่าน -> จบ)
  - การดึงข้อมูลสภาพอากาศปัจจุบันจาก API (ร้องขอ -> ได้ข้อมูลอุณหภูมิ -> จบ)
  - การดาวน์โหลดไฟล์รูปภาพ 1 รูปมาแสดงผลบนหน้าจอ
- สถานการณ์ที่เหมาะกับ Stream:
  - ระบบห้องแชท (Chat Application) ที่หน้าจอต้องคอยรับข้อความใหม่ๆ ที่เด้งเข้ามาจาก Firebase Firestore ตลอดเวลา
  - การฟังค่าจากเซนเซอร์ของมือถือ เช่น ค่าพิกัด GPS ที่เปลี่ยนไปเรื่อยๆ ขณะผู้ใช้กำลังเดิน (Geolocator Location Stream)
  - การฟังสถานะการเชื่อมต่ออินเทอร์เน็ต (Connectivity Status) เพื่อแจ้งเตือนผู้ใช้ทันทีเมื่อเน็ตหลุดหรือกลับมาใช้งานได้
---

## ข้อผิดพลาดที่พบบ่อย

| Error Message | สาเหตุ | วิธีแก้ |
|---|---|---|
| `A value of type 'Null' can't be assigned...` | กำหนด null ให้ตัวแปร non-nullable | เพิ่ม `?` หรือกำหนดค่าเริ่มต้น |
| `The getter '...' isn't defined` | เรียก method ที่ไม่มีใน Type นั้น | ตรวจสอบ Type ของตัวแปร |
| `Non-abstract class '...' missing concrete implementation` | ไม่ได้ implement abstract method | เพิ่ม `@override` และ implement method |
| `Uncaught Error: ...` | Future throw Error แต่ไม่มี try/catch | ห่อด้วย try/catch |
| `setState() or markNeedsBuild()...` (บน Flutter) | เรียก setState หลัง dispose | เช็ค `if (mounted)` ก่อน setState |

---

*ใบงานการทดลองที่ 2-1 | Dart Programming*
*วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่*
