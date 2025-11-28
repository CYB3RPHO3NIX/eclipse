# Eclipse

A Windows XP-themed UI component library with authentic Luna visual styles.

## Directory Structure

```
eclipse/
├── css/
│   └── eclipse.css    # Main CSS framework file
├── index.html         # Demo/documentation page
└── README.md
```

## Features

- **Windows XP Luna Theme** - Authentic Windows XP visual styling with classic gradients
- **Primary, Secondary, Success, Danger, and Warning** button variants in XP colors
- **Outline buttons** for flat Windows XP style
- **Size variants** (Small, Medium, Large) matching XP proportions
- **TextBox and TextArea** controls with sunken 3D appearance
- **Checkboxes** with classic XP styling and green checkmarks
- **Radio Buttons** with circular design and green selection indicators
- **Disabled state** support with authentic XP disabled appearance
- **Hover and active states** mimicking real Windows XP control behavior
- **Tahoma font** - The classic Windows XP system font
- **3D effects** with proper inset shadows and border styling

## Installation

Include the CSS file in your HTML:

```html
<link rel="stylesheet" href="css/eclipse.css">
```

## Usage

Open `index.html` in your browser to see all UI control styles.

### Button Classes

| Class | Description |
|-------|-------------|
| `.btn` | Base button class (required) - Windows XP gray button |
| `.btn-primary` | Luna Blue primary button |
| `.btn-secondary` | Classic gray secondary button |
| `.btn-success` | Green success button (Olive theme inspired) |
| `.btn-danger` | Red danger button |
| `.btn-warning` | Orange/yellow warning button |
| `.btn-outline-*` | Flat outline variant of any button |
| `.btn-sm` | Small button size |
| `.btn-lg` | Large button size |

### Form Control Classes

| Class | Description |
|-------|-------------|
| `.textbox` | Text input field with XP sunken border |
| `.textbox-sm` | Small text input |
| `.textbox-lg` | Large text input |
| `.textarea` | Multi-line text area with XP styling |
| `.checkbox-wrapper` | Container for custom checkbox |
| `.checkbox` | Custom checkbox visual element |
| `.checkbox-label` | Label text for checkbox |
| `.radio-wrapper` | Container for custom radio button |
| `.radio` | Custom radio button visual element |
| `.radio-label` | Label text for radio button |
| `.form-section` | Section container for form controls |
| `.form-group` | Group container for label and control |

### Examples

#### Buttons

```html
<button class="btn btn-primary">OK</button>
<button class="btn btn-secondary">Cancel</button>
```

#### TextBox

```html
<input type="text" class="textbox" placeholder="Enter text...">
<input type="text" class="textbox" disabled value="Disabled">
```

#### TextArea

```html
<textarea class="textarea" placeholder="Enter comments..."></textarea>
```

#### Checkbox

```html
<label class="checkbox-wrapper">
    <input type="checkbox" checked>
    <span class="checkbox"></span>
    <span class="checkbox-label">Option 1</span>
</label>
```

#### Radio Button

```html
<label class="radio-wrapper">
    <input type="radio" name="group1" checked>
    <span class="radio"></span>
    <span class="radio-label">Option A</span>
</label>
<label class="radio-wrapper">
    <input type="radio" name="group1">
    <span class="radio"></span>
    <span class="radio-label">Option B</span>
</label>
```

## Theme

This library is styled to match the Windows XP Luna Blue theme, featuring:
- Classic XP window chrome with gradient title bar
- Traditional beige/cream client area background (`#ece9d8`)
- Gradient buttons with 3D lighting effects
- Sunken text fields with proper border styling
- Custom checkboxes and radio buttons with green indicators
- Authentic focus indicators with dotted outline