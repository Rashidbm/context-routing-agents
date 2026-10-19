# Copy-ready team polls

Run the first two polls in the team chat. A short deadline helps settle ownership without a meeting. The lead makes the final assignment if preferences overlap, considering availability and the skills each member wants to practice.

## Poll 1 — preferred workstream

**Question:** أي جزء تفضّل تكون مسؤولًا عنه في مشروع Context Routing؟ اختر خيارًا واحدًا، وأرسل حساب GitHub مع إجابتك. إذا تكرر الاختيار سنوزع حسب الرغبة الثانية والوقت المتاح.

- Trace/SDK — تسجيل خطوات الايجنت وبناء مكتبة الربط ([#2](../../issues/2))
- Failure/Lessons — اكتشاف الخطأ المتكرر ومراجعة الدروس ([#3](../../issues/3))
- Runtime Router — تحديد وقت تطبيق الدرس وربطه بالايجنت ([#4](../../issues/4))
- Evaluation — تجهيز المهام والتجارب وتحليل النتائج ([#5](../../issues/5))

Ask each member to send a second choice in text. The lead owns coordination and integration ([#1](../../issues/1)).

## Poll 2 — weekly availability

**Question:** كم ساعة تقريبًا تقدر تخصص للمشروع كل أسبوع خلال الفترة القادمة؟ نستخدم الإجابة لتوزيع المهام بشكل عادل وتحديد نطاق النسخة الأولى.

- 2–3 hours
- 4–6 hours
- 7+ hours

This is for planning, not performance ranking. Confirm the course deadline separately if one exists.

## Poll 3 — example scenario (optional)

The default is a research agent with local search and fallback search, because it demonstrates the repeated empty-result loop using synthetic data. Use this poll only if the team wants to choose the scenario together; keep the underlying architecture unchanged.

**Question:** أي سيناريو نستخدم في الديمو الأول؟

- Research agent: نتيجة بحث فارغة ثم fallback (الاقتراح الافتراضي)
- Support agent: بحث في قاعدة معرفة ثم تصعيد للحالة المناسبة
- Other: اكتب سيناريو فيه خطأ متكرر وقياس نجاح واضح

## After the polls

Post the final owner list as a comment on [issue #1](../../issues/1), assign issues #2–#5, and ask each owner to post their first small deliverable on their issue. Avoid starting parallel implementations with different event or lesson schemas.
