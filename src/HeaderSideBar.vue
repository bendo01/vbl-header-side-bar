<template>
    <div>
        <nav class="nav background-is-black-transparent is-fixed-nav">
            <div class="nav-left">
                <a class="nav-item color-is-white">
                    <slot name="header"></slot>
                </a>
            </div>
            <div class="nav-right">
                <a class="nav-item color-is-white full-height-percent margin-left-auto" id="menu-open" v-on:click="toogleSideBarMenu">
                    <span class="icon margin-left-auto"><i class="fa fa-fw fa-bars"></i></span>
                </a>
            </div>
        </nav>
        <aside class="background-is-black-transparent side-navigation right" id="side-navigation" v-show="isActive">
            <div class="tabs is-centered margin-bottom-none background-is-black-transparent">
                <ul>
                    <li class="is-active">
                        <a class="trigger-tab color-is-purple" id="trigger-tab-menu"><i class="fa fa-fw fa-map-signs" aria-hidden="true"></i></a>
                    </li>
                    <li>
                        <a class="trigger-tab color-is-turquoise" id="trigger-tab-chatbot"><i class="fa fa-fw fa-microphone" aria-hidden="true"></i></a>
                    </li>
                </ul>
                <a class="close-side-bar-button color-is-danger" id="menu-close"><i class="fa fa-fw fa-close" aria-hidden="true" v-on:click="toogleSideBarMenu"></i></a>
            </div>
            <side-bar-tabs @toogleSideBarMenu="toogleSideBarMenu">
                <slot name="menu-list"></slot>
            </side-bar-tabs>
        </aside>
    </div>
</template>

<script>
'use strict';
var $ = require('jquery/dist/jquery.slim');
require('perfect-scrollbar/jquery')($);
import SideBarTab from './SideBarTab.vue';
import SideBarTabs from './SideBarTabs.vue';
export default {
    data () {
        return {
            isActive: false
        };
    },
    mounted () {
        $('#container-menu').perfectScrollbar();
    },
    components: {
        SideBarTab, SideBarTabs
    },
    methods: {
        toogleSideBarMenu () {
            $('#container-menu').perfectScrollbar();
            this.isActive = !this.isActive;
            if (this.isActive) {
                $('#container-menu').perfectScrollbar();
                $('html').addClass('is-clipped');
            } else {
                $('#container-menu').perfectScrollbar('destroy');
                $('html').removeClass('is-clipped');
            }
        }
    }
};
</script>

<style lang="scss">
@mixin scrollbar-rail-default($theme) {
  display: none;
  position: absolute; /* please don't change 'position' */
  opacity: map_get($theme, rail-default-opacity);
  transition: background-color .2s linear, opacity .2s linear;
}

@mixin scrollbar-rail-hover($theme) {
  background-color: map_get($theme, rail-hover-bg);
  opacity: map_get($theme, rail-hover-opacity);
}

@mixin scrollbar-default($theme) {
  position: absolute; /* please don't change 'position' */
  background-color: map_get($theme, bar-container-hover-bg);
  border-radius: map_get($theme, border-radius);
  transition: background-color .2s linear, height .2s linear, width .2s ease-in-out,
              border-radius .2s ease-in-out;
}

@mixin scrollbar-hover($theme) {
  background-color: map_get($theme, bar-hover-bg);
}

@mixin in-scrolling($theme) {
  &.ps-in-scrolling {
    &.ps-x > .ps-scrollbar-x-rail {
      @include scrollbar-rail-hover($theme);
      > .ps-scrollbar-x {
        @include scrollbar-hover($theme);
        height: map_get($theme, scrollbar-x-hover-height);
      }
    }
    &.ps-y > .ps-scrollbar-y-rail {
      @include scrollbar-rail-hover($theme);
      > .ps-scrollbar-y {
        @include scrollbar-hover($theme);
        width: map_get($theme, scrollbar-y-hover-width);
      }
    }
  }
}

// Layout and theme mixin
@mixin ps-container($theme) {
  -ms-touch-action: auto;
  touch-action: auto;
  overflow: hidden !important;
  -ms-overflow-style: none;

  // Edge
  @supports (-ms-overflow-style: none) {
    overflow: auto !important;
  }
  // IE10+
  @media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {
    overflow: auto !important;
  }

  &.ps-active-x > .ps-scrollbar-x-rail,
  &.ps-active-y > .ps-scrollbar-y-rail {
    display: block;
    background-color: map_get($theme, bar-bg);
  }

  @include in-scrolling($theme);

  > .ps-scrollbar-x-rail {
    @include scrollbar-rail-default($theme);
    bottom: map_get($theme, scrollbar-x-rail-bottom); /* there must be 'bottom' for ps-scrollbar-x-rail */
    height: map_get($theme, scrollbar-x-rail-height);

    > .ps-scrollbar-x {
      @include scrollbar-default($theme);
      bottom: map_get($theme, scrollbar-x-bottom); /* there must be 'bottom' for ps-scrollbar-x */
      height: map_get($theme, scrollbar-x-height);
    }
    &:hover,
    &:active {
      > .ps-scrollbar-x {
        height: map_get($theme, scrollbar-x-hover-height);
      }
    }
  }

  > .ps-scrollbar-y-rail {
    @include scrollbar-rail-default($theme);
    right: map_get($theme, scrollbar-y-rail-right); /* there must be 'right' for ps-scrollbar-y-rail */
    width: map_get($theme, scrollbar-y-rail-width);

    > .ps-scrollbar-y {
      @include scrollbar-default($theme);
      right: map_get($theme, scrollbar-y-right); /* there must be 'right' for ps-scrollbar-y */
      width: map_get($theme, scrollbar-y-width);
    }
    &:hover,
    &:active {
      > .ps-scrollbar-y {
        width: map_get($theme, scrollbar-y-hover-width);
      }
    }
  }

  &:hover {
    @include in-scrolling($theme);

    > .ps-scrollbar-x-rail,
    > .ps-scrollbar-y-rail {
      opacity: map_get($theme, rail-container-hover-opacity);
    }

    > .ps-scrollbar-x-rail:hover {
      @include scrollbar-rail-hover($theme);

      > .ps-scrollbar-x {
        @include scrollbar-hover($theme);
      }
    }

    > .ps-scrollbar-y-rail:hover {
      @include scrollbar-rail-hover($theme);

      > .ps-scrollbar-y {
        @include scrollbar-hover($theme);
      }
    }
  }
}

$ps-border-radius: 6px !default;

$ps-rail-default-opacity: 0 !default;
$ps-rail-container-hover-opacity: 0.6 !default;
$ps-rail-hover-opacity: 0.9 !default;

$ps-bar-bg: transparent !default;
$ps-bar-container-hover-bg: rgba(37, 49, 56, .9) !default;
$ps-bar-hover-bg: rgba(37, 49, 56, .9) !default;
$ps-rail-hover-bg: rgba(37, 49, 56, .9) !default;

// Sizes
$ps-scrollbar-x-rail-bottom: 0px !default;
$ps-scrollbar-x-rail-height: 6px !default;
$ps-scrollbar-x-bottom: 2px !default;
$ps-scrollbar-x-height: 6px !default;
$ps-scrollbar-x-hover-height: 11px !default;

$ps-scrollbar-y-rail-right: 0 !default;
$ps-scrollbar-y-rail-width: 15px !default;
$ps-scrollbar-y-right: 2px !default;
$ps-scrollbar-y-width: 6px !default;
$ps-scrollbar-y-hover-width: 11px !default;

$ps-theme-default: (
  border-radius: $ps-border-radius,
  rail-default-opacity: $ps-rail-default-opacity,
  rail-container-hover-opacity: $ps-rail-container-hover-opacity,
  rail-hover-opacity: $ps-rail-hover-opacity,
  bar-bg: $ps-bar-bg,
  bar-container-hover-bg: $ps-bar-container-hover-bg,
  bar-hover-bg: $ps-bar-hover-bg,
  rail-hover-bg: $ps-rail-hover-bg,
  scrollbar-x-rail-bottom: $ps-scrollbar-x-rail-bottom,
  scrollbar-x-rail-height: $ps-scrollbar-x-rail-height,
  scrollbar-x-bottom: $ps-scrollbar-x-bottom,
  scrollbar-x-height: $ps-scrollbar-x-height,
  scrollbar-x-hover-height: $ps-scrollbar-x-hover-height,
  scrollbar-y-rail-right: $ps-scrollbar-y-rail-right,
  scrollbar-y-rail-width: $ps-scrollbar-y-rail-width,
  scrollbar-y-right: $ps-scrollbar-y-right,
  scrollbar-y-width: $ps-scrollbar-y-width,
  scrollbar-y-hover-width: $ps-scrollbar-y-hover-width,
);

// Default theme
.ps-container {
  @include ps-container($ps-theme-default);
}

$sidebar-width: 285px !default;
$sidebar-z-index: 34 !default;
.side-navigation {
    background-color: white;
    display: flex;
    height: 100vh;
    width: $sidebar-width;
    flex-direction: column;
    z-index: $sidebar-z-index;
    overflow: hidden;
    position: fixed;
    top: 0;
    &.left {
        left: 0;
        right: auto;
    }

    &.right {
        left: auto;
        right: 0;
    }
}

.close-side-bar-button {
    position: absolute;
    right: 0.8125em;
    top: 0.625em;
    border-bottom: 0px !important;
}

.menu {
    padding: 7px;
    position: relative;
    height: 93vh;
    .menu-label {
        i {
            padding-top: 3px;
        }
    }
    .menu-list {
        li {
            a {
                &:hover {
                    background-color: inherit;
                }
                &.is-active {
                    background-color: inherit;
                }
                i {
                    padding-top: 3px;
                }
            }
            ul {
                padding-left: 12px !important;
                border: 0px !important;
            }
        }
    }
}
</style>
