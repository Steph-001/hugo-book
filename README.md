# Hugo Website with Hugo-Book Theme

This project implements a responsive educational website using Hugo with the hugo-book theme, featuring custom shortcodes for multimedia content and quiz functionality.

## Features

- **Responsive Design**: Adapts to different screen sizes (mobile, tablet, desktop)
- **Custom Shortcodes**:
  - `audio.html`: For embedding audio files
  - `video.html`: For embedding video files
  - `image.html`: For responsive images with captions
  - `gallery.html`: For image galleries with lightbox
  - `quiz.html`: For interactive quizzes
  - `youtube.html`: For embedding YouTube videos
- **Chapter Layout**: Blog-like pages for educational content
- **Sidebar Structure**: Left sidebar for general table of contents, right sidebar for page-specific table of contents

## Directory Structure

```
hugo-site/
├── assets/
│   └── custom.scss           # Custom responsive styles
├── content/
│   └── docs/                 # Educational content
│       ├── sample-chapter.md # Sample chapter with multimedia
│       └── sample-youtube.md # Sample chapter with YouTube videos
├── layouts/
│   ├── _default/
│   │   ├── list.html         # Template for chapter lists
│   │   └── single.html       # Template for individual chapters
│   ├── partials/
│   │   └── docs/
│   │       └── inject/
│   │           └── head.html # Custom head content
│   └── shortcodes/
│       ├── audio.html        # Audio shortcode
│       ├── gallery.html      # Gallery shortcode
│       ├── image.html        # Image shortcode
│       ├── quiz.html         # Quiz shortcode
│       ├── video.html        # Video shortcode
│       └── youtube.html      # YouTube shortcode
├── static/
│   ├── audio/                # Audio files
│   ├── images/               # Images and gallery
│   │   └── gallery/          # Gallery images
│   └── video/                # Video files
└── config.toml               # Hugo configuration
```

## Usage Examples

### Audio Shortcode

```
{{< audio src="/audio/sample.mp3" title="Sample Audio" >}}
```

### Video Shortcode

```
{{< video src="/video/sample.mp4" title="Sample Video" poster="/images/video-poster.jpg" >}}
```

### Image Shortcode

```
{{< image src="/images/sample.jpg" alt="Sample Image" caption="This is a sample image with a caption" >}}
```

### Gallery Shortcode

```
{{< gallery dir="images/gallery" caption=true >}}
```

### Quiz Shortcode

```
{{< quiz 
  question="What is the capital of France?" 
  options="London|Paris|Berlin|Madrid" 
  answer=1
  explanation="Paris is the capital and most populous city of France."
>}}
```

### YouTube Shortcode

```
{{< youtube "dQw4w9WgXcQ" title="Sample YouTube Video" >}}
```

## Installation

1. Clone the hugo-book theme:
   ```
   git clone https://github.com/alex-shpak/hugo-book themes/hugo-book
   ```

2. Start the Hugo server:
   ```
   hugo server -D
   ```

## Customization

- Modify `config.toml` to change site settings
- Edit `assets/custom.scss` to adjust responsive design
- Create new content in the `content/docs/` directory

## color schemes
- create a file in `assets/themes/` (e.g. `_blue-night.scss)
- add the mixin and root declaration:

  `@mixin theme-blue-night {
  // (paste theme code here)
}

:root {
  @include theme-blue-night;
}`
- update config.toml:
[params]
  BookTheme = 'blue-night'

  ## Design more broadly

  
1.Use the assets directory: Continue using your custom.scss file in the assets directory to override theme styles without modifying the original theme files.
2.Create template overrides: You can copy any template file from the theme's layouts directory to your site's layouts directory with the same path, and Hugo will use your version instead.
  For example: copy themes/hugo-book/layouts/_default/list.html to layouts/_default/list.html to customize it
3.Use Hugo's partial system: Create custom partials in layouts/partials/ to override specific components.
4.Understand the cascade:
  - CSS in custom.scss overrides theme styles
  - Templates in your layouts directory override theme templates
  - The module approach you're using makes this clean and         maintainable
