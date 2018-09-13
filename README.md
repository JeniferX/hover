# hover
shopify-hover
/*============================================================================
  Minimal | A theme by Shopify
  Built on Timber v2.0.0
==============================================================================*/

/*================ Variables, theme settings, and Sass mixins from Timber ================*/
/*================ Global | Sass Mixins ================*/
@mixin clearfix() {
  &:after {
    content: "";
    display: table;
    clear: both; }
  *zoom: 1;
}

@mixin prefix($property, $value) {
  -webkit-#{$property}: #{$value};
  -moz-#{$property}: #{$value};
  -ms-#{$property}: #{$value};
  -o-#{$property}: #{$value};
  #{$property}: #{$value};
}

/*============================================================================
  Prefix mixin for generating vendor prefixes.
  Based on https://github.com/thoughtbot/bourbon/blob/master/app/assets/stylesheets/addons/_prefixer.scss
  Usage:
    // Input:
    .element {
      @include prefix(transform, scale(1), ms webkit spec);
    }
    // Output:
    .element {
      -ms-transform: scale(1);
      -webkit-transform: scale(1);
      transform: scale(1);
    }
==============================================================================*/
@mixin prefixFlex($property, $value, $prefixes) {
  @each $prefix in $prefixes {
    @if $prefix == webkit {
      -webkit-#{$property}: $value;
    } @else if $prefix == moz {
      -moz-#{$property}: $value;
    } @else if $prefix == ms {
      -ms-#{$property}: $value;
    } @else if $prefix == o {
      -o-#{$property}: $value;
    } @else if $prefix == spec {
      #{$property}: $value;
    } @else  {
      @warn 'Unrecognized prefix: #{$prefix}';
    }
  }
}

@mixin transition($transition: 0.1s all) {
  @include prefix('transition', #{$transition});
}

@mixin transform($transform: 0.1s all) {
  @include prefix('transform', #{$transform});
}

@mixin gradient($from, $to, $fallback) {
  background: $fallback;
  background: -moz-linear-gradient(top, $from 0%, $to 100%);
  background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,$from), color-stop(100%,$to));
  background: -webkit-linear-gradient(top, $from 0%, $to 100%);
  background: -o-linear-gradient(top, $from 0%, $to 100%);
  background: -ms-linear-gradient(top, $from 0%, $to 100%);
  background: linear-gradient(top bottom, $from 0%, $to 100%);
}

@mixin backface($visibility: hidden) {
  @include prefix('backface-visibility', #{$visibility});
}

@mixin visuallyHidden {
  clip: rect(0 0 0 0);
  clip: rect(0, 0, 0, 0);
  overflow: hidden;
  position: absolute;
  height: 1px;
  width: 1px;
}

@mixin box-sizing($box-sizing: border-box) {
  -webkit-box-sizing: #{$box-sizing};
  -moz-box-sizing: #{$box-sizing};
  box-sizing: #{$box-sizing};
}

@function em($target, $context: $baseFontSize) {
  @if $target == 0 {
    @return 0;
  }
  @return $target / $context + 0em;
}

@function color-control($color) {
  @if (lightness( $color ) > 50) {
    @return #000;
  }
  @else {
    @return #fff;
  }
}

/*============================================================================
  Dependency-free breakpoint mixin
    - http://blog.grayghostvisuals.com/sass/sass-media-query-mixin/
==============================================================================*/
$min: min-width;
$max: max-width;
@mixin at-query ($constraint, $viewport1, $viewport2:null) {
  @if $constraint == $min {
    @media screen and ($min: $viewport1) {
      @content;
    }
  } @else if $constraint == $max {
    @media screen and ($max: $viewport1) {
      @content;
    }
  } @else {
    @media screen and ($min: $viewport1) and ($max: $viewport2) {
      @content;
    }
  }
}

/*============================================================================
  Accent text
==============================================================================*/

@mixin accentFontStack {
  font-size: em($accentFontSize);
  font-family: $accentFontStack;
  font-weight: $accentFontWeight;
  font-style: normal;
  @if $typeAccentSpacing {
    letter-spacing: 0.1em;
  }
  @if $typeAccentTransform {
    text-transform: uppercase;
  }
}

/*============================================================================
  Flexbox prefix mixins from Bourbon
    https://github.com/thoughtbot/bourbon/blob/master/app/assets/stylesheets/css3/_flex-box.scss
==============================================================================*/
@mixin display-flexbox() {
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  width: 100%; // necessary for ie10
}

@mixin flex-wrap($value: nowrap) {
  @include prefixFlex(flex-wrap, $value, webkit moz ms spec);
}

@mixin flex-direction($value) {
  @include prefixFlex(flex-direction, $value, webkit moz ms spec);
}

@mixin align-items($value: stretch) {
  $alt-value: $value;

  @if $value == 'flex-start' {
    $alt-value: start;
  } @else if $value == 'flex-end' {
    $alt-value: end;
  }

  // sass-lint:disable no-misspelled-properties
  -ms-flex-align: $alt-value;
  @include prefixFlex(align-items, $value, webkit moz ms o spec);
}

@mixin flex($value) {
  @include prefixFlex(flex, $value, webkit moz ms spec);
}

@mixin flex-basis($width: auto) {
  // sass-lint:disable no-misspelled-properties
  -ms-flex-preferred-size: $width;
  @include prefixFlex(flex-basis, $width, webkit moz spec);
}

@mixin align-self($align: auto) {
  // sass-lint:disable no-misspelled-properties
  -ms-flex-item-align: $align;
  @include prefixFlex(align-self, $align, webkit spec);
}

@mixin justify-content($justify: flex-start) {
  @include prefixFlex(justify-content, $justify, webkit ms spec);
}

{% if settings.enable_wide_layout %}
  $siteWidth: 1340px;
  $slideHeight: 486px;
{% else %}
  $siteWidth: 1030px;
  $slideHeight: 368px;
{% endif %}

$gutter: 30px;
$gridGutter: 30px;
$gridGutterMobile: 22px;
$radius: 2px;

$section-spacing: 55px;
$section-spacing-small: 35px;

$small: 480px;
$medium: 768px;
$large: 769px;
{% if settings.enable_wide_layout %}
$wide: 1250px;
{% endif %}

$viewportIncrement: 1px;
$postSmall: $small + $viewportIncrement;
$preMedium: $medium - $viewportIncrement;
$preLarge: $large - $viewportIncrement;

/*================ The following are dependencies of csswizardry grid ================*/
$breakpoints: (
  'small' '(max-width: #{$small})',
  'medium' '(min-width: #{$postSmall}) and (max-width: #{$medium})',
  'medium-down' '(max-width: #{$medium})',
  {% if settings.enable_wide_layout %}
  'large' '(min-width: #{$large}) and (max-width: #{$wide})',
  {% else %}
  'large' '(min-width: #{$large})',
  {% endif %}
  'post-large' '(min-width: #{$large})'{% if settings.enable_wide_layout %},
  'wide' '(min-width: #{$wide})'{% endif %}
);
$breakpoint-has-widths: ('small', 'medium', 'medium-down', 'large', 'post-large', 'wide');
$breakpoint-has-push:  ('medium', 'medium-down', 'large', 'post-large', 'wide');
$breakpoint-has-pull:  ('medium', 'medium-down', 'large', 'post-large', 'wide');

/*================ Color variables ================*/
$colorPrimary: {{ settings.color_primary }};
$colorSecondary: {{ settings.color_secondary }};

// Button colors
$colorBtnPrimary: $colorPrimary;
$colorBtnPrimaryHover: lighten($colorBtnPrimary, 10%);
$colorBtnPrimaryActive: darken($colorBtnPrimaryHover, 10%);
$colorBtnPrimaryText: {{ settings.color_button_primary_text }};

$colorBtnSecondary: $colorSecondary;
$colorBtnSecondaryHover: lighten($colorBtnSecondary, 10%);
$colorBtnSecondaryActive: darken($colorBtnSecondaryHover, 10%);
$colorBtnSecondaryText: {{ settings.color_button_secondary_text }};

$colorBtnTertiary: {{ settings.color_body_bg }};
$colorBtnTertiaryHover: $colorPrimary;
$colorBtnTertiaryActive: darken($colorPrimary, 10%);
$colorBtnTertiaryText: $colorPrimary;

// Text link colors
$colorLink: $colorPrimary;
$colorLinkHover: lighten($colorPrimary, 15%);

// Text colors
$colorTextBody: {{ settings.color_body_text }};
$colorTopBarText: {{ settings.color_topbar_text }};

// Backgrounds
$colorBody: {{ settings.color_body_bg }};
$colorHeader: transparent;
$colorTopBar: {{ settings.color_topbar_bg }};

// Background images
{% if settings.bg_custom != blank %}
$bgImage: '{{ settings.bg_custom | img_url: '1024x' }}';
{% elsif settings.theme_bg_image %}
  $bgImage: '{{ 'bg-music.png' | asset_url }}';
{% else %}
  $bgImage: false;
{% endif %}

$bgImageDisplay: {{ settings.bg_image_display }};
$passwordBgImage: '{{ 'password-page-background.jpg' | asset_url }}';

// Border colors
$colorBorder: {{ settings.color_borders }};

// Nav and dropdown link background
$colorNavText: {{ settings.color_header_text }};

// Site Footer
$colorFooterBg: {{ settings.color_footer_bg }};
$colorFooterText: {{ settings.color_footer_text }};
$colorFooterSocialLink: {{ settings.color_footer_social_link }};

// Helper colors
$disabledGrey: #f6f6f6;
$disabledBorder: darken($disabledGrey, 25%);
$errorRed: #d02e2e;
$errorRedBg: #fff6f6;
$successGreen: #56ad6a;
$successGreenBg: #ecfef0;

// Password page
$passwordPageUseBgImage: true;

/*================ Typography variables ================*/
{% if settings.type_base_family contains 'Google' %}
  {% assign type_base_parts = settings.type_base_family | split: '_' %}
  {% assign type_base_name = type_base_parts[1] %}
  {% capture base_family %}"{{ type_base_name | split: ':' | first | replace: '+', ' ' }}"{% if type_base_parts.last == 'serif' %}, serif {% else %}, "HelveticaNeue", "Helvetica Neue", sans-serif{% endif %}{% endcapture %}
{% else %}
  {% assign base_family = settings.type_base_family %}
{% endif %}

{% if settings.type_header_family contains 'Google' %}
  {% assign type_header_parts = settings.type_header_family | split: '_' %}
  {% assign type_header_name = type_header_parts[1] %}
  {% capture header_family %}"{{ type_header_name | split: ':' | first | replace: '+', ' ' }}"{% if type_header_parts.last == 'serif' %}, serif {% else %}, "HelveticaNeue", "Helvetica Neue", sans-serif{% endif %}{% endcapture %}
  {% assign header_weight = type_header_parts[2] %}
{% else %}
  {% assign header_family = settings.type_header_family %}
  {% assign header_weight = 700 %}
{% endif %}

{% if settings.type_accent_family contains 'Google' %}
  {% assign type_accent_parts = settings.type_accent_family | split: '_' %}
  {% assign type_accent_name = type_accent_parts[1] %}
  {% capture accent_family %}"{{ type_accent_name | split: ':' | first | replace: '+', ' ' }}"{% if type_accent_parts.last == 'serif' %}, serif {% else %}, "HelveticaNeue", "Helvetica Neue", sans-serif{% endif %}{% endcapture %}
  {% assign accent_weight = type_accent_parts[2] %}
{% else %}
  {% assign accent_family = settings.type_accent_family %}
  {% assign accent_weight = 400 %}
{% endif %}

// Body Font
$bodyFontStack: {{ base_family }};
$baseFontSize: {{ settings.type_base_size }};

// Header Font
$headerFontStack: {{ header_family }};
$headerFontWeight: {{ header_weight }};
$headerBaseFontSize: {{ settings.type_header_size }};

// Navigation and buttons
$accentFontStack: {{ accent_family }};
$accentFontWeight: {{ accent_weight }};
$accentFontSize: {{ settings.type_accent_size }};

// Header type settings
{% if settings.type_accent_spacing %}
  $typeAccentSpacing: true;
{% else %}
  $typeAccentSpacing: false;
{% endif %}

{% if settings.type_accent_transform %}
  $typeAccentTransform: true;
{% else %}
  $typeAccentTransform: false;
{% endif %}

$colorBlankstate: rgba($colorTextBody, 0.35);
$colorBlankstateBorder: rgba($colorTextBody, 0.15);
$colorBlankstateBackground: rgba($colorTextBody, 0.05);

.placeholder-svg, .icon--placeholder {
  display: block;
  fill: $colorBlankstate;
  background-color: $colorBlankstateBackground;
  width: 100%;
  height: 100%;
  max-width: 100%;
  max-height: 100%;
  border: 1px solid $colorBlankstateBorder;
}

.placeholder-noblocks {
  padding: 40px;
  text-align: center;
}

// Mimic a background image by wrapping the placeholder svg with this class
.placeholder-background {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  .icon {
    border: 0;
  }
}

// Custom styles for some blank state images
.image-bar__content .placeholder-image {
  position: absolute;
  top: 0;
  left: 0;
}

.flexslider .placeholder-svg {
  @include at-query($min, $medium) {
    height: $slideHeight;
  }
}

// Overwrite height settings for collection grid.
.grid-link__image-centered {
  .placeholder-svg {
    height: initial;
    max-height: initial;
  }
}


/*================ Vendor-specific styles ================*/
/* Magnific Popup CSS */
.mfp-bg {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1042;
  overflow: hidden;
  position: fixed;
  background: $colorBody;
  opacity: 1;
  filter: alpha(opacity=100); }

.mfp-wrap {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1043;
  position: fixed;
  outline: none !important;
  -webkit-backface-visibility: hidden; }

.mfp-container {
  text-align: center;
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  padding: 0 8px;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box; }

.mfp-container:before {
  content: '';
  display: inline-block;
  height: 100%;
  vertical-align: middle; }

.mfp-align-top .mfp-container:before {
  display: none; }

.mfp-content {
  position: relative;
  display: inline-block;
  vertical-align: middle;
  margin: 0 auto;
  text-align: left;
  z-index: 1045; }

.mfp-inline-holder .mfp-content, .mfp-ajax-holder .mfp-content {
  width: 100%;
  cursor: auto; }

.mfp-ajax-cur {
  cursor: progress; }

.mfp-zoom-out-cur, .mfp-zoom-out-cur .mfp-image-holder .mfp-close {
  cursor: -moz-zoom-out;
  cursor: -webkit-zoom-out;
  cursor: zoom-out; }

.mfp-zoom {
  cursor: pointer;
  cursor: -webkit-zoom-in;
  cursor: -moz-zoom-in;
  cursor: zoom-in; }

.mfp-auto-cursor .mfp-content {
  cursor: auto; }

.mfp-close, .mfp-arrow, .mfp-preloader, .mfp-counter {
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none; }

.mfp-loading.mfp-figure {
  display: none; }

.mfp-hide {
  display: none !important; }

.mfp-preloader {
  color: #CCC;
  position: absolute;
  top: 50%;
  width: auto;
  text-align: center;
  margin-top: -0.8em;
  left: 8px;
  right: 8px;
  z-index: 1044; }
  .mfp-preloader a {
    color: #CCC; }
    .mfp-preloader a:hover {
      color: #FFF; }

.mfp-s-ready .mfp-preloader {
  display: none; }

.mfp-s-error .mfp-content {
  display: none; }

button.mfp-close, button.mfp-arrow {
  overflow: visible;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
  display: block;
  outline: none;
  padding: 0;
  z-index: 1046;
  -webkit-box-shadow: none;
  box-shadow: none; }
button::-moz-focus-inner {
  padding: 0;
  border: 0; }

.mfp-close {
  width: 44px;
  height: 44px;
  line-height: 44px;
  position: absolute;
  right: 0;
  top: 0;
  text-decoration: none;
  text-align: center;
  opacity: 0.65;
  filter: alpha(opacity=65);
  padding: 0 0 18px 10px;
  color: $colorTextBody;
  font-style: normal;
  font-size: 28px;
  font-family: Arial, Baskerville, monospace; }
  .mfp-close:hover, .mfp-close:focus {
    opacity: 1;
    filter: alpha(opacity=100); }
  .mfp-close:active {
    top: 1px; }

.mfp-close-btn-in .mfp-close {
  color: #333; }

.mfp-image-holder .mfp-close, .mfp-iframe-holder .mfp-close {
  color: #FFF;
  right: -6px;
  text-align: right;
  padding-right: 6px;
  width: 100%; }

.mfp-counter {
  position: absolute;
  top: 0;
  right: 0;
  color: #CCC;
  font-size: 12px;
  line-height: 18px;
  white-space: nowrap; }

.mfp-arrow {
  position: absolute;
  opacity: 0.65;
  filter: alpha(opacity=65);
  margin: 0;
  top: 50%;
  margin-top: -55px;
  padding: 0;
  width: 90px;
  height: 110px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0); }
  .mfp-arrow:active {
    margin-top: -54px; }
  .mfp-arrow:hover, .mfp-arrow:focus {
    opacity: 1;
    filter: alpha(opacity=100); }
  .mfp-arrow:before, .mfp-arrow:after, .mfp-arrow .mfp-b, .mfp-arrow .mfp-a {
    content: '';
    display: block;
    width: 0;
    height: 0;
    position: absolute;
    left: 0;
    top: 0;
    margin-top: 35px;
    margin-left: 35px;
    border: medium inset transparent; }
  .mfp-arrow:after, .mfp-arrow .mfp-a {
    border-top-width: 13px;
    border-bottom-width: 13px;
    top: 8px; }
  .mfp-arrow:before, .mfp-arrow .mfp-b {
    border-top-width: 21px;
    border-bottom-width: 21px;
    opacity: 0.7; }

.mfp-arrow-left {
  left: 0; }
  .mfp-arrow-left:after, .mfp-arrow-left .mfp-a {
    border-right: 17px solid #FFF;
    margin-left: 31px; }
  .mfp-arrow-left:before, .mfp-arrow-left .mfp-b {
    margin-left: 25px;
    border-right: 27px solid #3F3F3F; }

.mfp-arrow-right {
  right: 0; }
  .mfp-arrow-right:after, .mfp-arrow-right .mfp-a {
    border-left: 17px solid #FFF;
    margin-left: 39px; }
  .mfp-arrow-right:before, .mfp-arrow-right .mfp-b {
    border-left: 27px solid #3F3F3F; }

.mfp-iframe-holder {
  padding-top: 40px;
  padding-bottom: 40px; }
  .mfp-iframe-holder .mfp-content {
    line-height: 0;
    width: 100%;
    max-width: 900px; }
  .mfp-iframe-holder .mfp-close {
    top: -40px; }

.mfp-iframe-scaler {
  width: 100%;
  height: 0;
  overflow: hidden;
  padding-top: 56.25%; }
  .mfp-iframe-scaler iframe {
    position: absolute;
    display: block;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.6);
    background: #000; }

/* Main image in popup */
img.mfp-img {
  width: auto;
  max-width: 100%;
  height: auto;
  display: block;
  line-height: 0;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  padding: 40px 0 40px;
  margin: 0 auto; }

/* The shadow behind the image */
.mfp-figure {
  line-height: 0; }
  .mfp-figure:after {
    content: '';
    position: absolute;
    left: 0;
    top: 40px;
    bottom: 40px;
    display: block;
    right: 0;
    width: auto;
    height: auto;
    z-index: -1;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.6);
    background: #444; }
  .mfp-figure small {
    color: #BDBDBD;
    display: block;
    font-size: 12px;
    line-height: 14px; }
  .mfp-figure figure {
    margin: 0; }

.mfp-bottom-bar {
  margin-top: -36px;
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  cursor: auto; }

.mfp-title {
  text-align: left;
  line-height: 18px;
  color: #F3F3F3;
  word-wrap: break-word;
  padding-right: 36px; }

.mfp-image-holder .mfp-content {
  max-width: 100%; }

.mfp-gallery .mfp-image-holder .mfp-figure {
  cursor: pointer; }

@media screen and (max-width: 800px) and (orientation: landscape), screen and (max-height: 300px) {
  /**
       * Remove all paddings around the image on small screen
       */
  .mfp-img-mobile .mfp-image-holder {
    padding-left: 0;
    padding-right: 0; }
  .mfp-img-mobile img.mfp-img {
    padding: 0; }
  .mfp-img-mobile .mfp-figure:after {
    top: 0;
    bottom: 0; }
  .mfp-img-mobile .mfp-figure small {
    display: inline;
    margin-left: 5px; }
  .mfp-img-mobile .mfp-bottom-bar {
    background: rgba(0, 0, 0, 0.6);
    bottom: 0;
    margin: 0;
    top: auto;
    padding: 3px 5px;
    position: fixed;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box; }
    .mfp-img-mobile .mfp-bottom-bar:empty {
      padding: 0; }
  .mfp-img-mobile .mfp-counter {
    right: 5px;
    top: 3px; }
  .mfp-img-mobile .mfp-close {
    top: 0;
    right: 0;
    width: 35px;
    height: 35px;
    line-height: 35px;
    background: rgba(0, 0, 0, 0.6);
    position: fixed;
    text-align: center;
    padding: 0; }
 }

@media all and (max-width: 900px) {
  .mfp-arrow {
    -webkit-transform: scale(0.75);
    transform: scale(0.75); }

  .mfp-arrow-left {
    -webkit-transform-origin: 0;
    transform-origin: 0; }

  .mfp-arrow-right {
    -webkit-transform-origin: 100%;
    transform-origin: 100%; }

  .mfp-container {
    padding-left: 6px;
    padding-right: 6px; }
 }

.mfp-ie7 .mfp-img {
  padding: 0; }
.mfp-ie7 .mfp-bottom-bar {
  width: 600px;
  left: 50%;
  margin-left: -300px;
  margin-top: 5px;
  padding-bottom: 5px; }
.mfp-ie7 .mfp-container {
  padding: 0; }
.mfp-ie7 .mfp-content {
  padding-top: 44px; }
.mfp-ie7 .mfp-close {
  top: 0;
  right: 0;
  padding-top: 0; }


/*================ Theme-specific partials ================*/
h1 {
  font-size: em($headerBaseFontSize);
  line-height: 1.2;
}

h2 {
  font-size: em(floor($headerBaseFontSize * 0.88)); //28px
  line-height: 1.3;
}

h3 {
  font-size: em(floor($headerBaseFontSize * 0.7)); //22px
  line-height: 1.4;
}

h4,
.tags {
  font-size: em(floor($headerBaseFontSize * 0.5)); //16px
  line-height: 1.6;
}

h4 {
  font-size: em(floor($headerBaseFontSize * 0.5)); //16px
  font-weight: bold;
}

h5 {
  font-size: em(floor($headerBaseFontSize * 0.5)); //16px
  line-height: 1.6;
}

h6 {
  font-size: em(floor($headerBaseFontSize * 0.45)); //14px
  line-height: 1.7;
}

.h1 { @extend h1; }
.h2 { @extend h2; }
.h3 { @extend h3; }
.h4 { @extend h4; }
.h5 { @extend h5; }
.h6 { @extend h6; }

/*================ Footer ================*/
.site-footer {
  p,
  li,
  .rte,
  input {
    font-size: 0.85em;
  }
}

.main-content {
  margin-top: $gutter / 2;

  .template-index & {
     margin-top: 0;
  }
}

@if ($colorBody == $colorFooterBg) or ($colorFooterBg == rgba(0,0,0,0))  {
  .main-content {
    padding-bottom: 0;
    
    &:after {
      content: '';
      display: block;
      padding-top: $gutter * 2;
      border-bottom: 1px solid $colorBorder;
    }
  }
}

@if $bgImageDisplay == 'tile' and $bgImage != false {
  html, body { background: $colorBody url($bgImage) repeat scroll; }
}
@else if $bgImageDisplay == 'stretch' and $bgImage != false {
  html, body { background: $colorBody url($bgImage) no-repeat center center fixed; -webkit-background-size: cover; -moz-background-size: cover; -o-background-size: cover; background-size: cover; }
}
@else if $bgImage == false {
  html, body { background: $colorBody; }
}

/*================ Index sections ================*/
.index-section {
  padding-top: $section-spacing-small / 2;
  padding-bottom: $section-spacing-small / 2;

  @include at-query($min, $large) {
    padding-top: $section-spacing / 2;
    padding-bottom: $section-spacing / 2;
  }

  .shopify-section:first-child & {
    padding-top: 0;
    border-top: 0;
  }

  .shopify-section:last-child & {
    padding-bottom: 0;
  }
}


/*================ Module-specific styles ================*/
.header-bar {
  @include clearfix();
  font-family: $accentFontStack;
  font-size: em(14px);
  font-weight: 400;
  background-color: $colorTopBar;
  color: $colorTopBarText;
  padding-top: 2px;
  padding-bottom: 2px;
  text-align: center;

  @include at-query($min, $large) {
    text-align: right;
    padding-top: 8px;
    padding-bottom: 8px;
  }

  a,
  button {
    color: $colorTopBarText;

    &:hover,
    &:active,
    &:focus {
      outline-color: $colorTopBarText;
    }
  }

  .inline-list {
    margin-bottom: 0;

    li {
      margin-bottom: 0;
    }
  }
}

@include at-query($min, $large) {
  .header-bar__left {
    text-align: left;
    width: 33.33%;
  }

  .header-bar__right {
    width: 66.66%;
  }
}

.header-bar__module {
  margin-bottom: $gutter/2;

  .header-bar__right &:last-child {
    margin-bottom: 0;
  }

  @include at-query($min, $large) {
    display: inline-block;
    vertical-align: middle;
    text-align: left;
    margin-bottom: 0;
  }
}

.header-bar__module--list {
  list-style: none;
  margin: 0;

  li {
    display: inline-block;
    margin: 0;

    & + li {
      margin-left: 6px;
    }
  }
}

.cart-page-link {
  display: inline-block;
}

.header-bar__cart-icon {
  font-size: 1.4em;
  margin-right: 4px;
}

.hidden-count {
  display: none;
}

.header-bar__sep {
  display: none;

  @include at-query($min, $large) {
    color: $colorTopBarText;
    opacity: 0.4;
    display: inline-block;
    padding: 0 10px;
  }
}

.header-bar__message, .header-message {
  max-width: 100%;
  overflow: hidden;
}

.header-bar__search {
  @include clearfix();
  position: relative;
  background-color: #fff;
  border: 0 none;
  border-radius: $radius;
  min-width: 100px;

  @include at-query($min, $large) {
    max-width: 160px;
    margin-left: 20px;

    &:first-of-type {
      margin-left: 0;
    }
  }

  @include at-query($max, $medium) {
    margin: 12px 30px;
  }

  @include at-query($max, $small) {
    margin: 12px 15px;
  }

  form,
  input,
  button {
    margin-bottom: 0;
  }
}

.header-bar__search-input[type="search"] {
  display: block;
  width: 60%;
  float: right;
  background: transparent;
  border-color: transparent;
  padding: 5px 0;

  &:focus {
    background: transparent;
    border-color: transparent;
  }
}

.header-bar__search-submit {
  position: absolute;
  display: block;
  float: left;
  width: 40%;
  font-size: 16px;
  padding: 4px 0;
}

.supports-fontface {
  .header-bar__search-submit {
    width: 20%;
  }

  .header-bar__search-input[type="search"] {
    width: 100%;
    padding-left: 30px;
  }

  @include at-query($max, $medium) {
    .header-bar__search-form {
      position: relative;
    }

    .header-bar__search-submit {
      width: 35px;
      position: absolute;
      top: 0;
      left: 0;
    }

    .header-bar__search-input[type="search"] {
      width: 100%;
      padding-left: 35px;
    }
  }
}

.header-bar__search {
  .btn,
  .btn:hover,
  .btn:focus {
    background: transparent;
    color: #555;
  }
}

@if ( ($colorTopBar == $colorBody) or ($colorTopBar == rgba(0,0,0,0)) ) {
  .header-bar__search-input::-webkit-input-placeholder {
    color: $colorTopBarText;
  }
  .header-bar__search-input::-moz-placeholder {
    color: $colorTopBarText;
  }
  .header-bar__search-input:-ms-input-placeholder {
    color: $colorTopBarText;
  }
  .header-bar__search-input[type="search"] {
    background-color: rgba(0,0,0,0.03);
  }
  .header-bar__search:first-of-type .header-bar__search-input[type="search"] {
    background-color: transparent;
  }
  .header-bar__search:first-of-type .header-bar__search-input[type="search"]:focus {
    background-color: rgba(0,0,0,0.03);
  }
}

.announcement-bar--mobile {
  padding-top: 5px;
  padding-bottom: 5px;
}

/*================ Module | Grid Link ================*/
.grid-link__container {
  margin-bottom: -$gutter;
}

.grid-link,
.grid-link--focus {
  position: relative;
  display: block;
  padding-bottom: $gutter;
  line-height: 1.3;

  &:hover,
  &:active {
    .grid-link__image {
      opacity: 0.8;
    }
  }
}

.grid-link--focus {
  padding: $gutter / 1.5;
  box-shadow: 0px 1px 1px rgba(0,0,0,0.1);
  margin-bottom: $gutter;

  &:before {
    display: block;
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: $colorFooterBg;
    @include transition(all 0.08s ease-in);
  }

  &:hover,
  &:active {
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
}

.grid-link__image {
  position: relative;
  display: table;
  table-layout: fixed;
  width: 100%;
  margin: 0 auto ( $gutter / 3 );
  @include transition(all 0.08s ease-in);

  img {
    display: block;
    margin: 0 auto;
    max-width: 100%;
    max-height: 600px;
  }

  .list-view__product & {
    min-width: 130px;
  }
}

.grid-link__image-centered {
  display: table-cell;
  vertical-align: middle;
  width: 100%;
  overflow: hidden;
}

.grid-link__image-sold-out {
  img {
    .sold-out & {
      opacity: 0.4;
      filter: alpha(opacity=40);
    }
  }
}

.grid-link__title,
.grid-link__meta {
  position: relative;
  margin-bottom: 5px;
}

.grid-link__title {
  color: $colorTextBody;
  font-size: .9em;
  line-height: 1.4;
  font-weight: bold;
}

.grid-link__vendor {
  font-size: .85em;
  font-weight: normal;
}

.grid-link__meta {
  font-size: .75em;
  line-height: 1.5;
  color: lighten($colorTextBody, 10%);
}

.grid-link__sale_price {
  opacity: 0.95;
  filter: alpha(opacity=95);
}

.list-view__product {
  border-bottom: 1px solid $colorBorder;
  margin-bottom: $gutter / 3;
  padding-bottom: $gutter / 3;
}

$badgeSize: 60px;
.badge {
  display: table;
  position: absolute;
  width: $badgeSize;
  height: $badgeSize;
  background-color: $colorPrimary;
  color: $colorBtnPrimaryText;
  border-radius: 50%;
  text-transform: uppercase;
  font-weight: bold;
  text-align: center;
  font-size: em(12px);
  line-height: 1.1;
  z-index: 10;
}

.badge--sold-out {
  top: 50%;
  left: 50%;
  margin-top: -($badgeSize / 2);
  margin-left: -($badgeSize / 2);
  background-color: $colorBtnSecondary;
  color: $colorBtnSecondaryText;
}

.badge--sale {
  top: -($badgeSize / 5);
  right: -($badgeSize / 5);
}

.badge__text {
  display: table-cell;
  vertical-align: middle;
  padding: 2px 8px 0;
}

.badge__text--small {
  font-size: 8px;
  padding-top: 0;
}

.mobile-nav-trigger,
.mobile-cart-page-link {
  font-weight: bold;

  .icon {
    position: relative;
    top: -1px;
    vertical-align: middle;
    padding-right: 4px;
  }
}

.mobile-nav-trigger {
  display: block;
  float: left;
  background: none;
  border: 0 none;
  padding: 0;
  margin: 0;

  .icon {
    font-size: 1.4em;
  }
}

.mobile-cart-page-link {
  display: block;
  float: right;

  .header-bar__cart-icon {
    font-size: 1.4em;
  }

  .cart-count {
    &:before {
      display: inline;
      content: "(";
    }
    &:after {
      display: inline;
      content: ")";
    }
  }
}

.mobile-nav {
  display: none;
  list-style: none;
  text-align: left;
  margin: 0;

  li {
    margin: 0;
  }
}

.mobile-nav__link {
  display: block;
  border-top: 1px solid $colorTopBarText;
  border-color: rgba($colorTopBarText, 0.2);

  /*================ Can't always control anchor markup to add a class ================*/
  > a {
    display: block;
    padding: ($gutter / 2.5) ($gutter / 2);
    font-size: em(15px);
    font-family: $headerFontStack;
    font-weight: $headerFontWeight;
    text-transform: uppercase;

    @include at-query ($min, $small) {
      padding-left: $gutter;
      padding-right: $gutter;
    }
  }
}

.mobile-nav__sublist-expand,
.mobile-nav__sublist-contract {
  display: inline-block;
  font-size: 0.6em;
  vertical-align: middle;
  margin: -2px 0 0 4px;
}

.mobile-nav__sublist-contract {
  display: none;
}

.mobile-nav__sublist-trigger.is-active {
  .mobile-nav__sublist-contract {
    display: inline-block;
  }

  .mobile-nav__sublist-expand {
    display: none;
  }
}

.mobile-nav__sublist {
  list-style: none;
  margin: 0;
  display: none;
  background-color: $colorBody;

  .mobile-nav__sublist {
    margin-left: $gutter / 2;

    .mobile-nav__sublist-link a {
      border-top: none;
    }
  }
}

.mobile-nav__sublist-link {
  a {
    display: block;
    padding: ($gutter / 2.5) ($gutter / 2);
    color: $colorNavText;
    font-size: em(15px);
    font-family: $headerFontStack;
    border-top: 1px solid $colorBorder;

    @include at-query ($min, $small) {
      padding-left: $gutter;
      padding-right: $gutter;
    }

    &:hover {
      opacity: 1;
      color: $colorPrimary;
    }
  }
}

.newsletter-grid {
  display: flex;
  flex-wrap: wrap;
}

.newsletter-section {
  .grid-uniform {
    margin-left: 0;
  }

  #contact_form {
    margin-bottom: 0;
  }

  .section-header__title {
    margin-bottom: 0;
  }

  .section-header__title-spacing {
    margin-bottom: $gutter / 2;
  }
}

.newsletter-wrapper {
  .grid-uniform {
    margin-left: 0;
  }
}

.newsletter-grid__item {
  padding: 0;
}

.newsletter-content-wrapper {
  display: flex;
  justify-content: center;
  flex-direction: column;
  height: 100%;
  padding: 50px 15%;
}

.newsletter-content p {
  margin: 0;
}

.newsletter-section {
  .input-group {
    display: block;
  }
  
  .input-group-field {
    margin-bottom: 10px;
  }

  .errors {
    margin-bottom: 10px;
  }
}

/*================ Module | Promo images ================*/

.featured-images .grid__item {
  margin-bottom: $gutter / 2;
}

.collection__grid-image-wrapper {
  width: 100%;
  position: relative;
  margin: 0 auto;
}

.collection__grid-image {
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.custom-content {
  @include display-flexbox;
  @include align-items(stretch);
  @include flex-wrap(wrap);
  width: auto;
  margin-bottom: -$gridGutter;
  margin-left: -$gridGutter;

  @include at-query($max, $small) {
    margin-bottom: -$gridGutterMobile;
    margin-left: -$gridGutterMobile;
  }
}

.custom__item {
  @include flex(0 0 auto);
  margin-bottom: $gridGutter;
  padding-left: $gridGutter;
  max-width: 100%;

  @include at-query($max, $small) {
    @include flex(0 0 auto);
    padding-left: $gridGutterMobile;
    margin-bottom: $gridGutterMobile;

    &.small--one-half {
      @include flex(1 0 50%);
      max-width: 400px;
      margin-left: auto;
      margin-right: auto;
    }
  }
}

.custom__item-inner {
  position: relative;
  display: inline-block;
  text-align: left;
  max-width: 100%;
}

.custom__item-inner--video,
.custom__item-inner--html {
  display: block;
}

/*================ Flex item alignment ================*/
.align--top-middle {
  text-align: center;
}

.align--top-right {
  text-align: right;
}

.align--middle-left {
  @include align-self(center);
}

.align--center {
  @include align-self(center);
  text-align: center;
}

.align--middle-right {
  @include align-self(center);
  text-align: right;
}

.align--bottom-left {
  @include align-self(flex-end);
}

.align--bottom-middle {
  @include align-self(flex-end);
  text-align: center;
}

.align--bottom-right {
  @include align-self(flex-end);
  text-align: right;
}

.page-content__item:not(:first-child) {
  margin-top: 15px;
}

.feature-row {
  @include display-flexbox();
  @include justify-content(space-between);
  @include align-items(center);

  @include at-query ($max, $medium) {
    @include flex-wrap(wrap);
  }
}

.feature-row__item {
  @include flex(0 1 50%);

  @include at-query($max, $medium) {
    @include flex(1 1 100%);
    max-width: 100%;
  }
}

.feature-row__image-wrapper {
  position: relative;
  margin: 0 auto;
}

.feature-row__image {
  display: block;
  margin: 0 auto;

  @include at-query ($max, $medium) {
    order: 1;
  }

  .supports-js & {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
  }

  .no-js & {
    @include visuallyHidden();
  }
}

.feature-row__text {
  padding-top: $section-spacing-small;
  padding-bottom: $section-spacing-small;

  @include at-query ($max, $medium) {
    order: 2;
    padding-bottom: 0; // always last element on mobile
  }
}

@include at-query ($min, $large) {
  .feature-row__text--left {
    padding-left: $section-spacing-small;
  }

  .feature-row__text--right {
    padding-right: $section-spacing-small;
  }
}

@include at-query ($min, $large) {
  .featured-row__subtext {
    font-size: em($baseFontSize + 2);
  }
}

.featured-blog__post {
  margin-bottom: $gridGutter;

  @include at-query($max, $small) {
    margin-bottom: $gridGutter * 1.25;
  }

  .article__featured-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    display: block;
  }

  .article__featured-image-wrapper {
    position: relative;
    margin-bottom: $gridGutter;

    @include at-query($max, $small) {
      margin-bottom: $gridGutterMobile;
    }

    .no-js & {
      @include visuallyHidden();
    }
  }

  .rte {
    margin-top: $gridGutter * 0.75;
    @include at-query($max, $small) {
      margin-bottom: $gridGutterMobile * 0.75;
    }
  }

  .h3 {
    margin-top: -5px;
  }

  .featured-blog__meta {
    font-size: .85em;
    margin-bottom: -5px;
  }
}

.placeholder {
  .article__featured-link {
    margin-bottom: $gridGutter;

    @include at-query($max, $small) {
      margin-bottom: $gridGutterMobile;
    }
  }
}

.gallery__image-container {
  position: relative;
}

.gallery__image-wrapper {
  img {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    width: 100%;
  }

  .no-js & {
    @include visuallyHidden();
  }
}

/*================ Module | Product Lightbox ================*/

.mfp-bg {
  background-color: $colorBody;

  &.mfp-fade {
    -webkit-backface-visibility: hidden;
    opacity: 0;
    @include transition(all 0.3s ease-out);

    //background opacity after load
    &.mfp-ready {
      opacity: 1;
      filter: alpha(opacity=100);
    }


    &.mfp-removing {
      @include transition(all 0.3s ease-out);
      opacity: 0;
      filter: alpha(opacity=0);
    }
  }
}

.mfp-fade {
  &.mfp-wrap {
    .mfp-content {
      opacity: 0;
      @include transition(all 0.3s ease-out);
    }

    &.mfp-ready {
      .mfp-content {
        opacity: 1;
      }
    }

    &.mfp-removing {
        @include transition(all 0.3s ease-out);
      .mfp-content {
        opacity: 0;
      }

      button {
        opacity: 0;
      }
    }
  }

}

.mfp-counter {
  display: none;
}

.mfp-figure {
  .mfp-gallery .mfp-image-holder & {
    cursor: zoom-out;
  }

  &:after {
    box-shadow: none;
  }
}

.mfp-img {
  background-color: $colorBody;
}

button.mfp-close {
  margin: 30px;
  font-size: em(40px);
  font-weight: 300px;
  opacity: 1;
  filter: alpha(opacity=100);
  color: $colorTextBody;
}

button.mfp-arrow {
  top: 0;
  height: 100%;
  width: 20%;
  margin: 0;
  opacity: 1;
  filter: alpha(opacity=100);

  &:after,
  & .mfp-a {
    display: none;
  }

  &:before,
  & .mfp-b {
    display: none;
  }

  &:active {
    margin-top: 0;
  }
}

.mfp-chevron {
  position: absolute;
  pointer-events: none;

  &:before {
    content: '';
    display: inline-block;
    position: relative;
    vertical-align: top;
    height: 25px;
    width: 25px;
    border-style: solid;
    border-width: 4px 4px 0 0;
    @include transform(rotate(-45deg));
  }

  &.mfp-chevron-right {
    right: 55px;

    &:before {
      @include transform(rotate(45deg));
    }
  }

  &.mfp-chevron-left {
    left: 55px;

    &:before {
      @include transform(rotate(-135deg));
    }
  }
}


.lt-ie9 {
  .mfp-chevron:before,
  .mfp-chevron:after {
    content: " ";
    position: absolute;
    display: block;
    border-width: 0;
    width: 0;
    height: 0;
    top: 50%;
    margin-top: -25px;
    border-top: 25px solid transparent;
    border-bottom: 25px solid transparent;
  }

  .mfp-chevron:before {
    z-index: 5;
  }

  .mfp-chevron:after {
    z-index: 2;
  }

  .mfp-chevron-right:after {
    border-left: 25px solid $colorTextBody;
    left: 80%;
  }

  .mfp-chevron-right:before {
    border-left: 25px solid white;
    left: 80%;
  }

  .mfp-chevron-left:after {
    border-right: 25px solid $colorTextBody;
    right: 80%;
  }

  .mfp-chevron-left:before {
    border-right: 25px solid white;
    right: 80%;
  }
}

/*============================================================================
  #FlexSlider
    - jQuery FlexSlider v2.2.0 | http://www.woothemes.com/flexslider/
    - Contributing author: Tyler Smith (@mbmufffin)
==============================================================================*/
.flexslider {
  margin: 0;
  padding: 0;
}

.flexslider li {
  margin: 0;
  max-width: 100%;
}

.flexslider .slides > li {
  display: none; /* Hide the slides before the JS is loaded. Avoids image jumping */
  margin: 0;
  position: relative;
  @include backface();
}

.flexslider .slides img {
  max-width: 100%;
  margin: 0 auto;
  display: block;
}

.slides { @include clearfix; }
html[xmlns] .slides { display: block; }
* html .slides { height: 1%; }

/*================ No JS Fallback ================*/
.no-js .slides > li:first-child { display: block; }
.flexslider { position: relative; zoom: 1; }
.flex-viewport { max-height: 2000px; -webkit-transition: all 1s ease; -moz-transition: all 1s ease; -o-transition: all 1s ease; transition: all 1s ease; }
.loading .flex-viewport { max-height: 300px; }
.flexslider .slides { zoom: 1; }
.carousel li { margin-right: 5px; }

/*================ Direction Nav ================*/
.flex-direction-nav {
  margin: 0;
  padding: 0;
  list-style: none;
}

.flex-direction-nav { *height: 0; }
.flex-direction-nav a  {
  display: block;
  width: 45px;
  position: absolute;
  top: 0;
  bottom: 0;
  z-index: 10;
  overflow: hidden;
  opacity: 0;
  cursor: pointer;
  @include transition(all 0.3s ease 0.4s);
}

.flex-direction-nav .flex-disabled {
  opacity: 0!important;
  filter: alpha(opacity=0);
  cursor: default;
}

.flex-direction-nav a {
  text-indent: -9999px;
  background: {
    color: transparent;
    repeat: no-repeat;
    size: 20px auto;
  }

  /*================ Hide SVG arrows in oldIE ================*/
  .lte-ie9 & {
    display: none;
  }

  &.flex-prev {
    background-image: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxNy4xLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iMjIuM3B4IiBoZWlnaHQ9IjQwcHgiIHZpZXdCb3g9IjAgMCAyMi4zIDQwIiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDAgMCAyMi4zIDQwIiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGZpbGw9IiNEM0QzRDMiIGQ9Ik0xOC43LDBMMCwxOS43TDE4LjcsNDBjMCwwLDUuMi0xLDMuMS0zLjFTNS43LDE5LjcsNS43LDE5LjdzMTQtMTQuNSwxNi4xLTE2LjZTMTguNywwLDE4LjcsMHoiLz4NCjwvc3ZnPg0K");
    background-position: center center;
  }

  &.flex-next {
    background-image: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxNy4xLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iMjIuM3B4IiBoZWlnaHQ9IjQwcHgiIHZpZXdCb3g9IjAgMCAyMi4zIDQwIiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDAgMCAyMi4zIDQwIiB4bWw6c3BhY2U9InByZXNlcnZlIj4NCjxwYXRoIGZpbGw9IiNEM0QzRDMiIGQ9Ik0wLjUsMy4xYzIuMSwyLjEsMTYuMSwxNi42LDE2LjEsMTYuNlMyLjYsMzQuOCwwLjUsMzYuOVMzLjYsNDAsMy42LDQwbDE4LjctMjAuM0wzLjYsMEMzLjYsMC0xLjYsMSwwLjUsMy4xDQoJeiIvPg0KPC9zdmc+DQo=");
    background-position: center center;
  }
}

/*================ Control Nav ================*/
.flex-control-nav {
  position: absolute;
  bottom: $gutter/2;
  width: 100%;
  text-align: center;
  margin: 0;
  padding: 0;
  list-style: none;
  z-index: 2;

  li {
    margin: 0 4px;
    display: inline-block;
    zoom: 1;
    *display: inline;
    vertical-align: middle;
  }
}

.flex-control-paging li a {
  width: 12px;
  height: 12px;
  display: block;
  background-color: #ededed;
  cursor: pointer;
  text-indent: -9999px;
  border-radius: 20px;
  border: 2px solid #fff;

  &:hover {
    background-color: #333;
  }

  &.flex-active {
    background-color: #fff;
    border-color: $colorPrimary;
    cursor: default;
  }
}

.flex-control-thumbs {margin: 5px 0 0; position: static; overflow: hidden;}
.flex-control-thumbs li {width: 25%; float: left; margin: 0;}
.flex-control-thumbs img {width: 100%; display: block; opacity: .7; cursor: pointer;}
.flex-control-thumbs img:hover {opacity: 1;}
.flex-control-thumbs .flex-active {opacity: 1; cursor: default;}

.flexslider:hover {
  .flex-next,
  .flex-prev {
    opacity: 1;
    @include transition(all 0.3s ease);
  }
}

.flex-direction-nav .flex-prev { left: 20px; }
.flex-direction-nav .flex-next { right: 20px; }
.flexslider:hover .flex-prev { left: 0; }
.flexslider:hover .flex-next { right: 0; }

/*================ Custom Flexslider Styles ================*/
.flexslider .slides {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

.slide-link {
  display: block;

  img {
    display: block;
  }
}

/*================ Social share buttons ================*/
$shareButtonHeight: 22px;
$shareButtonCleanHeight: 30px;
$shareBorderColor: #ececec;

.social-sharing {
  font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;

  * {
    -webkit-box-sizing:border-box;
    -moz-box-sizing:border-box;
    box-sizing:border-box;
  }

  a {
    display: inline-block;
    color: #fff;
    border-radius: 2px;
    margin: 5px 10px 5px 0;
    height: $shareButtonHeight;
    line-height: $shareButtonHeight;
    text-decoration: none;
    font-weight: normal;

    &:hover {
      color: #fff;
    }
  }

  span {
    display: inline-block;
    vertical-align: top;
    height: $shareButtonHeight;
    line-height: $shareButtonHeight;
    font-size: 12px;
  }

  .icon {
    padding: 0 5px 0 10px;

    &:before {
      line-height: $shareButtonHeight;
    }
  }

  /*================ Large Buttons ================*/
  &.is-large a {
    height: $shareButtonHeight*2;
    line-height: $shareButtonHeight*2;

    span {
      height: $shareButtonHeight*2;
      line-height: $shareButtonHeight*2;
      font-size: 18px;
    }

    .icon {
      padding: 0 10px 0 18px;

      &:before {
        line-height: $shareButtonHeight*2;
      }
    }
  }
}

.share-title {
  font-weight: 900;
  font-size: 12px;
  padding-right: 10px;

  .is-large & {
    padding-right: 16px;
  }
}

.share-facebook {
  background-color: #3b5998;

  &:hover {
    background-color: darken(#3b5998, 10%);
  }
}

.share-twitter {
  background-color: #00aced;

  &:hover {
    background-color: darken(#00aced, 10%);
  }
}

.share-pinterest {
  background-color: #cb2027;

  &:hover {
    background-color: darken(#cb2027, 10%);
  }
}

/*================ Clean Buttons ================*/
.social-sharing.is-clean {
  a {
    background-color: #fff;
    border: 1px solid $shareBorderColor;
    color: #333;
    height: $shareButtonCleanHeight;
    line-height: $shareButtonCleanHeight;

    span {
      height: $shareButtonCleanHeight;
      line-height: $shareButtonCleanHeight;
      font-size: 13px;
    }

    &:hover {
      background-color: $shareBorderColor;
    }

    .share-title {
      font-weight: normal;
    }
  }

  .icon-facebook {
    color: #3b5998;
  }

  .icon-twitter {
    color: #00aced;
  }

  .icon-pinterest {
    color: #cb2027;
  }
}


/*================ View-specific styles ================*/
/*============= Templates | Password page =============*/

.template-password {
  height: 100vh;
  text-align: center;
}

.password-page__wrapper {
  display: table;
  height: 100%;
  width: 100%;

  @if $passwordPageUseBgImage {
    background-image: url($passwordBgImage);
    background-size: cover;
    background-repeat: no-repeat;
    color: #ffffff;
  } @else {
    color: $colorTextBody;
  }

  a {
    color: inherit;
  }

  hr {
    padding: ($gutter / 2) 0;
    margin: 0 auto;
    max-width: ($gutter * 2);

    @if $passwordPageUseBgImage {
      border-color: inherit;
    } @else {
      border-color: $colorBorder;
    }
  }

  .social-sharing {
    a {
      color: #fff;
    }

    &.is-clean a {
      color: #333;
      background: #fff;

      &:hover{
        background: #ececec;
      }
    }
  }
}

.password-header-section {
  display: table-row;
}

.password-page__header {
  display: table-cell;
  height: 1px;
}

.password-page__header__inner {
  padding: ($gutter / 2) $gutter;
}

.password-page__logo {
  margin-top: 3 * $gutter;

  @if $passwordPageUseBgImage {
    color: inherit;
  } @else {
    color: $colorNavText;
  }

  .logo {
    max-width: 100%;
  }
}

.password-page__main {
  display: table-row;
  width: 100%;
  height: 100%;
  margin: 0 auto;
}

.password-page__main__inner {
  display: table-cell;
  vertical-align: middle;
  padding: ( $gutter / 2 ) $gutter;
}

.password-page__hero {
  font-family: $headerFontStack;
  font-weight: $headerFontWeight;
  font-size: em(42px);
  line-height: 1.25;
  text-transform: none;
  letter-spacing: 0;
  text-rendering: optimizeLegibility;

  @include at-query($min, $postSmall) {
    font-size: em(60px);
  }

  @include at-query($min, $large) {
    font-size: em(64px);
  }
}

.password-page__message {
  font-style: italic;
  font-size: 120%;

  img {
    max-width: 100%;
  }
}

.password-page__message,
.password-page__login-form,
.password-page__signup-form {
  max-width: 500px;
  margin: 0 auto;
}

.password-page__message,
.password-page__login-form {
  text-align: center;
  padding: $gutter;
}

.password-page__login-form,
.password-page__signup-form {
  @include at-query($min, $small) {
    padding: 0 $gutter;
  }

  .input-group {
    width: 100%;
  }

  .errors ul {
    list-style-type: none;
    margin-left: 0;
  }
}

.lt-ie9 .template-password {
  .newsletter__submit-text--small,
  .password-page__login-form__submit-text--small {
    display: none !important;
  }
}

input[type="submit"].password-page__login-form__submit,
input[type="submit"].password-page__signup-form__submit {
  font-size: 0.9em;
}

.password-page__social-sharing {
  margin-top: $gutter;
}

.password-login,
.admin-login  {
  margin-top: $gutter / 2;
  a:hover {
    color: inherit;
  }
}

.password-login {
  font-family: $headerFontStack;
  font-size: em(14px);
  line-height: 14px;
}

.lock-icon-svg {
  width: 14px;
  height: 14px;
  display: inline-block;
  vertical-align: baseline;

  path {
    fill: currentColor;
  }

  /* Hiding the SVG logo in IE8 */
  .lt-ie9 & {
    display: none;
  }
}

.admin-login {
  font-size: 95%;
}

.password-page__footer {
  display: table-row;
  height: 1px;

  @if $passwordPageUseBgImage{
    color: inherit;
  } @else {
    color: $colorFooterText;
  }
}

.password-page__footer_inner {
  display: table-cell;
  vertical-align: bottom;
  padding: $gutter;
  line-height: 1.5 * $baseFontSize;
  font-size: 95%;
}

.shopify-link {
  color: inherit;

  &:hover {
    color: inherit;
  }
}

.shopify-logo-svg {
  width: 1.5 * $baseFontSize * 120 / 35;
  height: 1.5 * $baseFontSize;
  display: inline-block;
  line-height: 0;
  vertical-align: top;

  path {
    fill: currentColor;
  }

  /* Hiding the SVG logo in IE8, we show the word 'Shopify' instead */
  .lt-ie9 & {
    display: none;
  }
}

/* =========
   Hiding the word 'Shopify' but not from screen readers.
   IE8 does not support SVG, so in it we hide the logo and show the word.
   To target all browsers except IE8, we use the class 'modern',
   which needs to be added to the html element.
   ========= */

.shopify-name {
  .modern & {
    @include visuallyHidden;
  }
}

.search__image-wrapper {
  width: 100%;
  margin: 0 auto;

  &.supports-js {
    position: relative;
  }
}

.search__image {
  display: block;
  margin: 0 auto;

  &.lazyload {
    opacity: 0;
  }

  .supports-js & {
    position: absolute;
    top: 0;
    width: 100%
  }
}

