import streamlit as st
import nltk
from pyresparser import ResumeParser
import warnings
import re

warnings.filterwarnings("ignore", category=UserWarning)
nltk.download('stopwords', quiet=True)

st.title("AI-Powered Resume Reviewer")

uploaded_file = st.file_uploader("Upload Resume (PDF/DOCX)", type=['pdf', 'docx'])
job_keywords = st.text_area("Enter Job-Specific Keywords (comma-separated)", "Python, AI, data science")

if uploaded_file and job_keywords:
    with open("temp_resume.pdf" if uploaded_file.name.endswith('.pdf') else "temp_resume.docx", "wb") as f:
        f.write(uploaded_file.getbuffer())
    resume_file = "temp_resume.pdf" if uploaded_file.name.endswith('.pdf') else "temp_resume.docx"
    
    data = ResumeParser(resume_file).get_extracted_data()
    
    st.subheader("Extracted Resume Data")
    st.write(f"**Name:** {data.get('name', 'N/A')}")
    st.write(f"**Email:** {data.get('email', 'N/A')}")
    st.write(f"**Skills:** {', '.join(data.get('skills', []))}")
    st.write(f"**Experience:** {data.get('total_experience', 'N/A')}")
    st.write(f"**Education:** {data.get('degree', 'N/A')} from {data.get('college_name', 'N/A')}")
    
    keywords_list = [k.strip().lower() for k in job_keywords.split(',')]
    resume_skills = [s.lower() for s in data.get('skills', [])]
    
    matches = [k for k in keywords_list if any(k in skill for skill in resume_skills)]
    missing = [k for k in keywords_list if k not in matches]
    
    st.subheader("Keyword Matching")
    col1, col2 = st.columns(2)
    with col1:
        st.success(f"**Matches ({len(matches)}):** {', '.join(matches)}")
    with col2:
        st.error(f"**Missing ({len(missing)}):** {', '.join(missing)}")
    
    if missing:
        suggestions = [f"• Add '{kw}' to skills or experience section with examples." for kw in missing[:5]]
        st.subheader("Improvement Suggestions")
        for sug in suggestions:
            st.write(sug)
    else:
        st.success("Great match! Resume aligns well with job keywords.")
