# رسالة البداية للفريق

هذه رسالة متابعة لوصف الفكرة المرسل يوم 11 سبتمبر. أرسلها بعد نشر الوثائق، ثم أرسل [الاستطلاعين](team-polls.md) في محادثة الفريق.

> يا فريق، متابعة لفكرة **Context Routing for AI Agents** اللي أرسلتها يوم 11 سبتمبر: قررت نبدأ تنفيذها كمشروع مفتوح المصدر قريب من تجربة Mirrors. هدفنا أن يقدر مطوّر يربط agent عنده، يشوف الـtraces، يراجع lesson مقترحة، يعاين الحالات التي ستنطبق عليها، ثم يشغّل الـrouting ويقيس أثره.
>
> جهزت المستودع: https://github.com/Rashidbm/context-routing-agents . ابدأوا من README ثم [خطة المشروع والأدوار](https://github.com/Rashidbm/context-routing-agents/blob/main/docs/kickoff-plan.md). حددت للنسخة الأولى SDK بلغة Python، تجميعًا بسيطًا للأخطاء المتكررة، مراجعة واعتماد lessons، preview قبل التفعيل، router وقت التشغيل، dashboard محلية بسيطة، ومقارنة على مهام جديدة. سنستخدم agent واحدًا وديمو واحدًا أولًا حتى تشتغل الرحلة كاملة، ثم نوسع التكاملات.
>
> [ورقة Sentry](https://arxiv.org/abs/2610.02994) هي مرجعنا البحثي الأساسي. جهزت [مراجعة أدبيات](https://github.com/Rashidbm/context-routing-agents/blob/main/docs/literature-review.md) تلخص الورقة وما حولها، وما الذي سنأخذه أو نبسطه، وكيف نقيم عملنا. المطلوب من كل مسؤول قراءة الأقسام المرتبطة بجزئه وكتابة ثلاث نقاط في الـissue: فكرة نأخذها، شيء نبسطه، وشيء سنختبره. نتائج الورقة ليست نتائج مشروعنا؛ سننشر أرقامنا نحن.
>
> أنا أمسك [التنسيق، عقد البيانات، dashboard، والدمج](https://github.com/Rashidbm/context-routing-agents/issues/1). المسارات الأربعة الأخرى هي: [SDK وtraces](https://github.com/Rashidbm/context-routing-agents/issues/2)، [تحليل الفشل ودورة حياة lessons](https://github.com/Rashidbm/context-routing-agents/issues/3)، [router ومعاينة التطابق](https://github.com/Rashidbm/context-routing-agents/issues/4)، و[التقييم والمقارنة](https://github.com/Rashidbm/context-routing-agents/issues/5). كل مسؤول يكتب جزءه التقني ونتائجه للتقرير النهائي، ونراجع التقرير معًا.
>
> بما أننا ما نقدر نجتمع اليوم، بطرح استطلاعًا لاختيار المسار وآخر للوقت المتاح أسبوعيًا. أرسلوا حساب GitHub مع الاختيار الأول والثاني. بعد تثبيت المسؤوليات، كل شخص يكتب في الـissue أول تسليم صغير، وما يحتاجه من الأجزاء الثانية. نحدّث المهمة بصيغة `Done / Next / Blocked`، ونفتح Draft PR مبكرًا. أي قرار يغيّر واجهة مشتركة نوثقه في GitHub حتى يقدر الكل يشتغل من دون اجتماع.
>
> أول هدف مشترك: ديمو يعمل من trace إلى lesson معتمدة، preview، routing، ونتيجة قابلة للقياس. إذا احتجنا نخفف النطاق، نحافظ على هذا المسار كاملًا ونؤجل التحسينات الأخرى.
