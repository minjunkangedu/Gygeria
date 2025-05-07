from zipfile import ZipFile
import os

# Create project folder structure again after state reset
project_name = "krusty_krab_site"
project_dir = f"/mnt/data/{project_name}"
os.makedirs(project_dir, exist_ok=True)
html_path = os.path.join(project_dir, "index.html")

# Sample HTML content placeholder (final implementation will include full code)
html_content = """
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 사이트</title>
</head>
<body>
  <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
  <!-- 전체 기능은 여기 포함될 예정 -->
</body>
</html>
"""

# Write HTML to file
with open(html_path, "w", encoding="utf-8") as f:
    f.write(html_content)

# Create a zip file containing the project
zip_path = f"/mnt/data/{project_name}.zip"
with ZipFile(zip_path, 'w') as zipf:
    zipf.write(html_path, arcname=f"{project_name}/index.html")

zip_path
