As a teacher, after marking students’ writing-- whether by assigning scores or adding comments directly to their original Word documents-- you may wish to delay sharing this feedback immediately. This is often to pave the way for peer assessment or student-led group discussions: these activities allow students to exchange opinions, analyze each other’s work, and engage in collaborative dialogue without being influenced by your evaluations. To temporarily remove your marked content (e.g., scores, comments) from the documents before these activities, you can follow the steps below, using either method (2) or (3):

1) Make sure to install python-docx library using pip:

pip install python-docx

2) open each .docx file in a directory, remove the paragraph if it starts with "机评成绩", and then save the file.

import os
from docx import Document

directory = '/path/to/your/docx/files'  # specify your directory path

for filename in os.listdir(directory):
    if filename.endswith(".docx"):
        doc = Document(os.path.join(directory, filename))
        for para in doc.paragraphs:
            if para.text.startswith("机评成绩"):
                doc.paragraphs.remove(para)
        doc.save(os.path.join(directory, filename))


3) Create a new document and add the paragraphs we want to keep (because python-docx doesn't support paragraph removal). This will create a new document with the same content except the paragraph you want to delete. The new document will be saved in the same directory with 'new_' prefix in the filename.

import os
from docx import Document

directory = '/path/to/your/docx/files'  # specify your directory path

for filename in os.listdir(directory):
    if filename.endswith(".docx"):
        doc = Document(os.path.join(directory, filename))
        new_doc = Document()
        for para in doc.paragraphs:
            if not para.text.startswith("机评成绩"):
                new_doc.add_paragraph(para.text)
        new_doc.save(os.path.join(directory, 'new_'+filename))


Now I encounter a problem with the previous codes. The previous codes worked well with a paragraph. However now I want do remove a paragraph which starts with a line break symbol and then the first words "评语", and then ends with another line break symbol. The only difference from the previous codes are the paragraph is not marked by paragraphs symbols, instead, marked by line break symbols in .docx. How to do?

To handle paragraphs marked by line breaks within a paragraph, you can split the text and check each line. Once you encounter the line that starts with "评语", you can stop adding the lines to the new document. Here's how you can modify the script:

import os
from docx import Document

directory = "D:/10_Lidanli/He_Fabing"  # specify your directory path

for subdir, dirs, files in os.walk(directory):
    for file in files:
        if file.endswith(".docx"):
            filepath = subdir + os.sep + file
            doc = Document(filepath)
            new_doc = Document()
            for para in doc.paragraphs:
                lines = para.text.split('\n')  # split the paragraph into lines
                for line in lines:
                    if line.startswith("评语"):
                        break  # Stop adding lines to the new document
                    new_doc.add_paragraph(line)
            new_doc.save(os.path.join(subdir, 'new_'+file))

In this script, for each paragraph, it splits the paragraph into separate lines. If a line starts with "评语", it stops adding lines to the new document. The rest of the lines (before "评语") are added to the new document as separate paragraphs. The new document is then saved in the same directory with a 'new_' prefix.
