import streamlit as st
import os
from google import genai
from google.genai import types

# إعداد الصفحة
st.set_page_title("محلل مكالمات المبيعات الذكي")
st.set_page_layout("wide")

st.title("📞 محلل مكالمات المبيعات بالذكاء الاصطناعي")
st.write("قم برفع ملف نصي يحتوي على نص مكالمة المبيعات، وسيقوم النظام بتحليله واستخراج النقاط الهامة.")

# إدخال مفتاح الـ API
api_key = st.text_input("أدخل مفتاح Gemini API الخاص بك:", type="password")

# رفع ملف نصي للمكالمة
uploaded_file = st.file_uploader("اختر ملف نصي للمكالمة (TXT)", type=["txt"])

if uploaded_file is not None and api_key:
    # قراءة محتوى الملف
    call_text = uploaded_file.read().decode("utf-8")
    
    with st.expander("عرض نص المكالمة الأصلي"):
        st.text(call_text)
        
    if st.button("بدء التحليل الشامل", type="primary"):
        with st.spinner("جاري تحليل المكالمة باستخدام Gemini..."):
            try:
                # تهيئة عميل Gemini الجديد
                client = genai.Client(api_key=api_key)
                
                prompt = f"""
                قم بتحليل مكالمات المبيعات التالية بدقة واحترافية عالية. قدم التحليل باللغة العربية ويتضمن:
                1. ملخص تنفيذي للمكالمة.
                2. أبرز نقاط الألم واحتياجات العميل.
                3. الاعتراضات التي أبداها العميل وكيف تم التعامل معها.
                4. تقييم أداء مسؤول المبيعات (نقاط القوة والتحسين).
                5. الخطوات القادمة أو التوصيات لإغلاق الصفقة بنجاح.

                نص المكالمة:
                {call_text}
                """
                
                response = client.models.generate_content(
                    model='gemini-2.5-flash',
                    contents=prompt,
                )
                
                st.success("تم التحليل بنجاح!")
                st.markdown("### 📊 تقرير التحليل:")
                st.markdown(response.text)
                
            except Exception as e:
                st.error(f"حدث خطأ أثناء الاتصال بـ Gemini: {e}")
elif uploaded_file and not api_key:
    st.warning("الرجاء إدخال مفتاح Gemini API في الحقل بالأسفل لتفعيل التحليل.")
    



A simple Streamlit app template for you to modify!

[![Open in Streamlit](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://blank-app-template.streamlit.app/)

### How to run it on your own machine

Prerequisite: install `uv` if you don't already have it.

```
$ curl -LsSf https://astral.sh/uv/install.sh | sh
```

1. Sync the dependencies
streamlit run your_script.py
   ```
   $ uv sync
   ```

2. Run the app

   ```
   $ uv run streamlit run streamlit_app.py
   ```
