   - name: Inject HTML list into README and index
     run: |
       python3 - << 'EOF'
       import re
       import os

       with open("HTML_LIST.md", "r", encoding="utf-8") as f:
           html_list = f.read().rstrip()

       pattern = r"(<!-- HTML_LIST_START -->)(.*?)(<!-- HTML_LIST_END -->)"

       for filename in ("README.md", "index.md"):
           if not os.path.exists(filename):
               continue

           with open(filename, "r", encoding="utf-8") as f:
               text = f.read()

           new_text = re.sub(
               pattern,
               lambda m: m.group(1) + "\n\n" + html_list + "\n\n" + m.group(3),
               text,
               flags=re.S,
           )

           with open(filename, "w", encoding="utf-8") as f:
               f.write(new_text)
       EOF
