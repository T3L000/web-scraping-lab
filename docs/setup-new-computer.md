# Setup On A New Computer

这份说明用于在另一台电脑上继续学习。

## 1. Clone Repository

```powershell
git clone https://github.com/T3L000/web-scraping-lab.git
cd web-scraping-lab
```

## 2. Create Virtual Environment

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

## 3. Install Dependencies

```powershell
python -m pip install -r requirements.txt
```

## 4. Register Jupyter Kernel

```powershell
python -m ipykernel install --user --name web-scraping-lab --display-name "Web Scraping Lab"
```

## 5. Start Jupyter

```powershell
jupyter notebook .
```

打开 notebook 后，选择 `Web Scraping Lab` kernel。

## Daily Sync Rule

每次开始学习前：

```powershell
git pull
```

每次学习结束后：

```powershell
git status
git add .
git commit -m "exercises: add chapterXX practice"
git push
```

## What Stays Local

这些内容不提交到仓库：

- `.venv/`
- 原书 PDF
- 原书配套代码
- 大量抓取数据
- Cookie、token、账号密码
- Jupyter 自动缓存和临时 `Untitled*.ipynb`
