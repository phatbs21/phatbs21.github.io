---
title: Sử dụng Hugo & Obsidian & Github để viết blogs miễn phí bằng markdown text
description: Setup Hugo & Obsidian để viết blog dưới dạng markdown text
date: 2025-03-02 06:07:27+0700
image: cover.jpg
categories:
- Technical
- Hướng dẫn
tags:
- obsidian
- hugo
math: false
---

Trong bài viết này, mình sẽ hướng dẫn cách kết hợp [Obsidian](https://obsidian.md/) và [Hugo](https://gohugo.io/) để có thể viết blog bằng code markdown text một cách free và hiệu quả

## Obsidian

[Obsidian](https://obsidian.md/) là một app taking note sử dụng markdown text. Nó có thể quản lý file một cách linh hoạt và quản lý các file note một cách có cấu trúc.

Về cơ bản thì Obsidian hỗ trợ trên mọi nền tảng - trừ web app - thứ mà mình rất muốn có để có thể viết một cách dễ dàng trên chromebook của mình :(

Download obsidian tại trang chủ: https://obsidian.md/

## Hugo

[Hugo](https://gohugo.io/) là một website hỗ trợ người dùng tạo blogs bằng markdown text rất nhanh và gọn. Đồng thời hỗ trợ switch theme nhanh chóng. Hiện tại blogs của mình đang được sử dụng dựa trên nền tảng này

Bạn có thể vào trang chủ của Hugo rồi làm theo hướng dẫn để có thể cài đặt tuỳ theo hệ điều hành mà bạn sử dụng: https://gohugo.io/installation/

Với những bạn xài windows thì cách dễ nhất để cài là download file trực tiếp từ [release github của Hugo](https://github.com/gohugoio/hugo/releases) và download file (hugo_extended_withdeploy_x.xxx.x_windows-amd64.zip) và giải nén nó ra vào ổ đĩa C:/

Không thấy file hugo_extended ? Nó nằm ở đây:

![](IMG-20250302172135534.png)

Sau đó thêm C:\Hugo (Folder chứa hugo.exe) vào [environments path](https://vi.101-help.com/cach-them-bien-moi-truong-path-cua-windows-3f256ab251/) (system variables)

![](IMG-20250302172125329.png)

Trong phần prerequisites có yêu cầu tải thêm hai phần mềm khác để Hugo hoạt động là [Git](https://git-scm.com/downloads/win) và [Go](https://go.dev/dl/) để phần mềm hoạt động. Bạn nhớ tải và cài đặt nhé (next next next là được)

## Obsidian-export

[Obsidian-export](https://github.com/zoni/obsidian-export) dùng để có thể chuyển post từ obsidian sang hugo. Có thể bạn sẽ nghĩ rằng chỉ cần mở trực tiếp folder post trong Hugo để có thể chỉnh sửa và viết bài post mới là được rồi chứ cần việc chi mà sử dụng thêm một công cụ khác?

 > 
 > Hiện tại trong phiên bản obsidian mới đã cho phép tắt wikilink, mình vẫn sử dụng obsidian-export để đồng bộ hoá giữa folder valut/blogs và content/post. Bạn có thể sử dụng lệnh cp trên windows để thay thế cho Obsidian-export

Obsidian sử dụng một dạng markdown của riêng nó là Wikilink chứ không sử dụng dạng markdown thông thường. Ví dụ

Wikiling: `[[Three laws of motion]]`

Markdown: `[Three laws of motion](Three%20laws%20of%20motion.md)`

Vì vậy, Obsidian-export được sinh ra để chuyển từ dạng Wikilink sang dạng markdown thông thường - thứ mà hugo có thể hiểu được và nó cũng có thể giúp copy file từ vault của mình vào thẳng mục post mà không cần switch vault thủ công.

Download file [obsidian-export-x86_64-pc-windows-msvc](https://github.com/zoni/obsidian-export/releases)

## Github

Có một host và domain free với đuôi github.io đồng thời có github action giúp việc update bài lên được tự động hoá. Bài viết này giả định bạn đã biết sử dụng github. Với các bạn non-tech thì có thể kéo xuống cuối \[bài viết này\](# GitHub là gì? Giới thiệu tính năng và hướng dẫn cài đặt GitHub) để xem cách dùng

# Mục tiêu của việc combine những thứ này lại

1. Sử dụng một vault obsidian cho cả việc ghi chú riêng và post bài cho blog
1. Có một template để có thể viết bài cho blog mà không tốn công setup lại
1. Tự động public bài việc thông qua script
1. Có một blogs miễn phí của riêng mình được lưu trữ trên github

# Cài đặt

## Setup Hugo

Tạo một thư mục để chứa hugo site, đồng thời tạo trong vault trong obsidian một thư mục blogs (bạn có thể chọn một folder khác)

[Mở cmd](https://www.wikihow.vn/M%E1%BB%9F-th%C6%B0-m%E1%BB%A5c-trong-CMD) mà bạn mới tạo (hugo site)

````bash
hugo new site obsidian-hugo-demo
````

Push folder obsidian-hugo-demo lên github vào repo  \<Tên github của bạn>.github.io

Sau khi hoàn tất, bạn có thể sẽ cần một theme mới đẹp mắt cho blog của mình: [Theme](https://themes.gohugo.io/). Đây là một số theme đẹp có thể bạn sẽ thích: [Theme](https://www.reddit.com/r/gohugo/comments/xl3qca/what_is_your_goto_theme/). Sau đó đặt theme mình muốn vào folder theme của hugo:

````bash
git submodule add https://github.com/CaiJimmy/hugo-theme-stack themes/stack
````

Sau đó, bạn sẽ cần sửa file hugo.toml để nó có thể hoạt động trên github của bạn

````toml
baseURL = 'https://<username github của bạn>.github.io'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = "hugo-paper" // Bạn thay tên theme mà bạn đã download của bạn vào đây

[module]
[[module.mounts]]
  source = 'content'
  target = 'content'
  excludeFiles = ['templates/**']
````

Sau đó để hugo có thể đọc được file wikilink của obsidian thì bạn làm như sau (Trong trường hợp bạn không muốn dùng obsidian export mà muốn chỉnh sửa trực tiếp)

````bash
cd <Folder hugo site của bạn>
cd content
mkdir -p posts/assets
mkdir -p content/templates
mkdir -p layouts/_default/_markup
touch layouts/_default/_markup/render-image.html
touch layouts/_default/_markup/render-link.html
````

Chỉnh sửa file `layouts/_default/_markup/render-image.html`:

````html
{{- $url := urls.Parse .Destination -}}
{{- $scheme := $url.Scheme -}}

<a href="
  {{- if eq $scheme "" -}}
    {{- if strings.HasSuffix $url.Path ".md" -}}
      {{- relref .Page .Destination | safeURL -}}
    {{- else -}}
      {{- .Destination | safeURL -}}
    {{- end -}}
  {{- else -}}
    {{- .Destination | safeURL -}}
  {{- end -}}"
  {{- with .Title }} title="{{ . | safeHTML }}"{{- end -}}>
  {{- .Text | safeHTML -}}
</a>

{{- /* whitespace stripped here to avoid trailing newline in rendered result caused by file EOL */ -}}
````

Chỉnh sửa file `layouts/_default/_markup/render-image.html`:

````html
{{- $url := urls.Parse .Destination -}}
{{- $scheme := $url.Scheme -}}

<img src="
  {{- if eq $scheme "" -}}
    {{- if strings.HasSuffix $url.Path ".md" -}}
      {{- relref .Page .Destination | safeURL -}}
    {{- else -}}
      {{- printf "/%s%s" .Page.File.Dir .Destination | safeURL -}}
    {{- end -}}
  {{- else -}}
    {{- .Destination | safeURL -}}
  {{- end -}}"
  {{- with .Title }} title="{{ . | safeHTML }}"{{- end -}}
  {{- with .Text }} alt="{{ . | safeHTML }}"
  {{- end -}}
/>

{{- /* whitespace stripped here to avoid trailing newline in rendered result caused by file EOL */ -}}
````

Ref: [Relative links with Hugo](https://github.com/zoni/obsidian-export?tab=readme-ov-file#relative-links-with-hugo)

Vậy là đã xong phần hugo, tiếp theo mình sẽ setup và obsidian và obsidian-export để copy trực tiếp post mới từ trong \<vault riêng>/blogs

# Setup obsidian

Tạo folder blogs trong obsidian của bạn

![](IMG-20250302172059935.png)

Sau đó tắt wikilink và chỉnh location của new attachments vào thư mục hiện tại:

![](IMG-20250302171756674.png)

Và chỉnh lại template để thuận tiện thêm bài post mới trong tương lai (Bạn vui lòng chỉnh *Template* lại theo ý của bạn)

![](IMG-20250302172924610.png)

File template hiện tại mình đang sử dụng, bạn có thể kham khảo

````md
---

title: "{{Title}}"

description: Welcome to Hugo Theme Stack

date: "{{date:YYYY-MM-DD}} {{time:HH:mm:ss}}+0700"

image: cover.jpg

categories:

  - Example Category

tags:

  - Example

  - Tag

math: false

---
````

*Chú ý: ở mục date thì phần +0700 là múi giờ*

## Setup Obsidian-export & script để tự động post bài mới

Giải nén file obsidian-export và đặt ở một nơi bạn chắn chắn không xoá nhầm, sau đó copy path của file `obsidian-export.exe` `Path to obsidian your vault` `path to hugo\content\post`

Ví dụ:

````
"C:\Users\CVIP_LAB\Documents\Obsidian\obsidian-export-x86_64-pc-windows-msvc\obsidian-export.exe" C:\Users\CVIP_LAB\Documents\Obsidian\obsidian-vault\Blogs C:\Users\CVIP_LAB\Documents\phatbs21.github.io\content\post
cd /d C:\Users\CVIP_LAB\Documents\phatbs21.github.io
hugo
git pull origin master
git add .
git commit -m "new"
git push -u origin master

echo Done!
pause
````

Script tự động up bài viết mới sau khi viết bài trong obsidian. Bạn lưu code này dưới dạng file: `update.bat`

# Setup hiện tại của mình để viết bài

Đây là cách mình sử dụng để viết blogs trên máy tính cá nhân:

1. Viết và chỉnh sửa trực tiếp một bài post mới trong thư mục /blogs, những thư mục khác mình vẫn sử dụng để lưu note ghi chú riêng của mình
1. Chạy script tự động đẩy bài mới vào hugo và public lên website

Nếu bạn không đang sử dụng laptop của mình thì có thể sử dụng kết hợp [HackMD](https://hackmd.io/) và copy từ đó vào Codespace của github để thêm bài viết mới

1. Vào repo của blogs trên github và chọn code -> Create codespace on master

![IMG-20250302071259049.png](attachments\IMG-20250302071259049.png)

2. Vào thư mục content/post -> tạo một folder mới và copy toàn bộ file từ hugo vào đây (trong hình là ví dụ với folder hello_world)

![IMG-20250302072238586.png](attachments\IMG-20250302072238586.png)

3. Thêm message (Bắt buộc) -> commit -> yes -> sync changes

![IMG-20250302072425483.png](attachments\IMG-20250302072425483.png)

Với những bài viết lần sau, khi đã đã có sẵn codespace thì bạn nhớ pull nó xuống trước để tránh conflict nhé

![](IMG-20250302172031023.png)

Phù, vậy là xong rồi. Nếu bạn có thắc mắc thì thì comment bên dưới để mình giúp đỡ nhé
