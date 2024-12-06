# Pelican 主题 README

## 介绍

这是一个为 [Pelican](https://getpelican.com/) 静态站点生成器设计的主题。本主题旨在提供简洁、响应式的阅读体验，同时支持多种自定义选项以满足不同用户的个性化需求。

## 安装

1. **下载主题**:
   - 从 GitHub 下载或克隆此仓库到您的 Pelican 内容目录下的 `themes` 文件夹中。
   ```bash
   git clone https://github.com/your-repo/pelican-theme.git themes/your-theme-name
   ```

2. **配置 Pelican**:
   - 编辑您的 `pelicanconf.py` 文件，将主题设置为您刚刚下载的主题名称。
   ```python
   THEME = 'themes/your-theme-name'
   ```

## 使用

### 自定义配置

本主题支持以下配置项，您可以在 `pelicanconf.py` 中进行设置：

- **SITEURL**: 网站的基础 URL。
- **SITENAME**: 网站的名称。
- **SITESUBTITLE**: 网站的副标题。
- **DESCRIPTION**: 网站的描述。
- **AUTHOR**: 作者的名字。
- **SOCIAL**: 作者的社交媒体链接，格式为 `(平台名称, 链接)` 的列表。
- **MENUITEMS**: 导航菜单项，格式为 `(名称, 链接)` 的列表。
- **DISPLAY_PAGES_ON_MENU**: 是否在导航菜单中显示页面。
- **DISPLAY_CATEGORIES_ON_MENU**: 是否在导航菜单中显示分类。
- **THEME_STATIC_DIR**: 静态资源目录。
- **CSS_FILE**: 主 CSS 文件的路径。
- **JS_FILE**: 主 JS 文件的路径。

### 示例配置

```python
# 显示左侧导航
DISPLAY_LEFT_MENU = True

# 头部导航
SUB_MENUS = (
    ('博客', 'https://css9.com/', '_blank', 'io io-wordpress icon-fw icon-lg'),
    ('今日热点', 'https://css9.com/', '_self', 'iconfont icon-chart icon-fw icon-lg'),
)

# 底部菜单
FOOTER_MENUS = (
    ('css博客', 'https://css9.com/', '_blank'),
    ('765AI导航', 'https://765ai.com/', '_blank'),
)
# 左侧菜单
LEFTSIDE_MENUS = (
    ("AI热门推荐", "iconfont icon-hot icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI写作平台", "io io-change icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI绘画工具", "iconfont icon-picture icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI智能体平台", "io io-tuandui icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI智能对话", "iconfont icon-android icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI搜索引擎", "iconfont icon-search icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI设计工具", "io io-gongju icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI视频平台", "io io-caozuoshili icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI音频工具", "iconfont icon-notice icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI办公效率", "io io-sucai1-copy icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI学习资源", "io io-book icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI大模型平台", "io io-linggan icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI应用场景", "iconfont icon-chart-pc icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI行业应用", "iconfont icon-business icon-fw icon-lg",DISPLAY_LEFT_MENU),
    ("AI开发平台", "io io-changyongmokuai icon-fw icon-lg",False),
    ("AI基础设施", "io io-iowen icon-fw icon-lg",False),
)
# COPYRIGHT
COPYRIGHT = "Copyright ©2024 by css9"
# 底部链接
FRIENDS_LINK = (
    ("CSS9博客", "https://css9.com", "_blank"),
    ("粤ICP备20010391号-2", "https://beian.miit.gov.cn", "_blank"),
)
# 加载导航数据，导航数据保存在content下的csv文件
def load_nav_data():
    json_data = {}
    menu_titles = [menu[0] for menu in LEFTSIDE_MENUS]
    #print("菜单标题列表:", menu_titles)
    
    content_dir = 'content'
    if not os.path.exists(content_dir):
        print(f"错误: content目录不存在: {os.path.abspath(content_dir)}")
        return json_data
    
    # 遍历content目录下的所有子目录
    for menu_title in menu_titles:
        menu_dir = os.path.join(content_dir, menu_title)
        if not os.path.exists(menu_dir):
            print(f"警告: 目录不存在: {menu_dir}")
            continue
            
        #print(f"\n处理目录: {menu_title}")
        menu_index = menu_titles.index(menu_title)
        items = []
        print(f"\n处理目录: {menu_title},index:{menu_index}")
        # 查找目录下的CSV文件
        csv_files = [f for f in os.listdir(menu_dir) if f.endswith('.csv')]
        if not csv_files:
            print(f"警告: 在 {menu_dir} 中没有找到CSV文件")
            continue
            
        # 读取CSV文件（假设每个目录只有一个CSV文件）
        csv_path = os.path.join(menu_dir, csv_files[0])
        try:
            with open(csv_path, 'r', encoding='utf-8') as csvfile:
                reader = csv.DictReader(csvfile)
                #print(f"CSV列名: {reader.fieldnames}")
                for row in reader:
                    favicon = row.get('favicon', '')
                    if not favicon or 'NULL' in favicon.upper():
                        favicon = f"{SITEURL}/{THEME_STATIC_DIR}/images/favicon.png"
                    elif not favicon.startswith(('http://', 'https://')):
                        favicon = f"{SITEURL}/{THEME_STATIC_DIR}/{favicon}"
                    url = row.get('网址', '')
                    icon = favicon
                    name = row.get('标题', '')
                    desc = row.get('描述', '')
                    category = menu_title
                    item = {
                        'url': url,
                        'icon': icon,
                        'category': category,
                        'name': name,
                        'desc': desc,
                    }
                    items.append(item)
                    if name == 'NULL':
                        continue
            json_data[f'json_data_{menu_index}'] = items
            #print(f"已加载 {len(items)} 条数据到 json_data_{menu_index}")
        except Exception as e:
            print(f"处理文件 {csv_path} 时发生错误: {str(e)}")
    
    print("\n最终数据结构:", json_data.keys())
    return json_data

NAVIGATION_LINKS = load_nav_data()

# 导航详情页随机8条相关导航数据
def random_sample(value, count):
    import random
    if isinstance(value, dict):
        value = list(value.values())
    elif isinstance(value, set):
        value = list(value)
    return random.sample(value, min(count, len(value)))

# 获取文章分类
def get_categories(articles):
    categories = set()
    for article in articles:
        categories.add(article.category)
    return sorted(categories)

JINJA_FILTERS = {
    'get_categories': get_categories,
    'random_sample': random_sample,
}

# 底部自定义内容
EXTRA_HTML = '<div class>...</div>'

# 分享按钮
SOCIAL_SHARE_CODE = '<div class>...</div>'
```
## 贡献

欢迎贡献代码！如果您发现任何问题或有改进建议，请提交 Issue 或 Pull Request。

## 许可

本主题采用 MIT 许可证，详情见 [LICENSE](LICENSE) 文件。

## 联系

如果您有任何问题或建议，可以通过以下方式联系我：

- Email: cjd2015@qq.com.com
- GitHub: https://github.com/cssjidi/cjd-nav

---

希望您喜欢这个主题，并能用它创建出精彩的个人网站！