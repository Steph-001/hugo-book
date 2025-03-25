# Hugo Website Media Loading Issues - Solutions Documentation

## Issues Identified and Fixed

### 1. Missing Media Files
- **Problem**: Content files referenced media files that didn't exist in the expected locations
  - `/images/sample.jpg` referenced in sample-chapter.md was missing
  - Gallery directory referenced in content was missing
- **Solution**: 
  - Created missing image files by copying existing images
  - Created the gallery directory structure and populated it with sample images
  - Ensured all referenced media files exist in the correct locations

### 2. Go Module Configuration Issues
- **Problem**: The go.mod file contained an invalid Go version (1.22.2) that was incompatible with the installed Go version
- **Solution**: 
  - Updated go.mod file to use Go version 1.18 to match the installed Go version
  - This allowed Hugo modules to load correctly

### 3. Template Compatibility Issues
- **Problem**: Multiple template files had compatibility issues with the current Hugo version
  - CSS processing errors in html-head.html (css.Sass function not defined)
  - Brand template errors with .Sites.Default.Home.RelPermalink
  - Menu template errors with hugo.IsMultilingual
- **Solution**:
  - Created local template overrides in the project's layouts directory:
    - Updated html-head.html to use resources.ToCSS instead of css.Sass
    - Simplified brand.html to use .Site.Home.RelPermalink directly
    - Removed the multilingual check in menu.html

### 4. Shortcode Improvements
- **Problem**: The gallery shortcode didn't handle missing directories gracefully
- **Solution**:
  - Enhanced the gallery.html shortcode with error handling
  - Added a fileExists check to prevent errors when directories are missing
  - Improved path construction for image files
  - Added visual error feedback when gallery directories aren't found

## Best Practices for Avoiding Media Loading Issues

### 1. Media File Organization
- Always ensure media files exist in the locations referenced by content
- Use a consistent directory structure for different media types (images, audio, video)
- Consider using Hugo's asset pipeline for processing media files

### 2. Hugo Version Compatibility
- Check that your theme is compatible with your installed Hugo version
- Test with the same Hugo version in development and production environments
- When upgrading Hugo, review theme compatibility and update templates as needed

### 3. Template Management
- Create local template overrides instead of modifying theme files directly
- Test template changes thoroughly before deploying
- Document any template modifications for future reference

### 4. Shortcode Implementation
- Implement error handling in shortcodes to gracefully handle missing files
- Use conditional checks (fileExists) to prevent errors
- Provide meaningful error messages when resources aren't found

### 5. Go Module Management
- Ensure go.mod file uses a compatible Go version
- Keep module dependencies up to date
- Test module changes in a development environment before deploying

## Implementation Details

### Fixed File Structure
```
hugo-book/
├── assets/
│   ├── custom.scss
│   └── themes/
├── content/
│   └── docs/
│       ├── sample-chapter.md
│       └── sample-youtube.md
├── layouts/
│   ├── partials/
│   │   └── docs/
│   │       ├── brand.html (override)
│   │       ├── html-head.html (override)
│   │       └── menu.html (override)
│   └── shortcodes/
│       ├── audio.html
│       ├── gallery.html (improved)
│       ├── image.html
│       └── video.html
├── static/
│   ├── audio/
│   │   └── sample.mp3
│   ├── images/
│   │   ├── damian.jpeg
│   │   ├── sample.jpg (created)
│   │   └── gallery/ (created)
│   │       ├── image1.jpg
│   │       ├── image2.jpg
│   │       └── image3.jpg
│   └── video/
│       └── sample.mp4
└── go.mod (fixed version)
```

### Key Code Changes

#### 1. Fixed go.mod
```
module github.com/steph/prems

go 1.18

require github.com/alex-shpak/hugo-book v0.0.0-20250203221943-645c868cec13 // indirect
```

#### 2. Improved Gallery Shortcode
```html
{{ $dir := .Get "dir" }}
{{ $class := .Get "class" | default "gallery" }}
{{ $height := .Get "height" | default "200px" }}
{{ $caption := .Get "caption" | default true }}

<div class="{{ $class }}">
  {{ $path := print "static/" $dir }}
  {{ if (fileExists $path) }}
    {{ range (readDir $path) }}
      {{ $imgFile := printf "/%s/%s" $dir .Name }}
      <div class="gallery-item">
        <a href="{{ $imgFile }}" data-lightbox="gallery" data-title="{{ .Name }}">
          <img src="{{ $imgFile }}" alt="{{ .Name }}" loading="lazy" />
        </a>
        {{ if $caption }}
          <div class="gallery-caption">{{ .Name }}</div>
        {{ end }}
      </div>
    {{ end }}
  {{ else }}
    <div class="gallery-error">Gallery directory not found: {{ $dir }}</div>
  {{ end }}
</div>
```

## Conclusion

The media loading issues in the Hugo website were successfully resolved by addressing multiple related problems: missing media files, template compatibility issues, and Go module configuration. By implementing the fixes and following the best practices outlined in this document, the website now correctly loads all media files and functions as expected.
