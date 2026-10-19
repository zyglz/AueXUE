<script setup lang="ts">
import { useThemeConfig } from '../node_modules/valaxy-theme-sakura/composables'

const themeConfig = useThemeConfig()
</script>

<template>
  <!-- 覆盖主题同名组件：移除默认的 SakuraHero（首页全屏图片/视频横幅），直接显示首页内容 -->
  <div class="sakura-home" flex="~ col center">
    <SakuraMultiColumns class="sakura-safe-padding sakura-home-layout sakura-site-content" base>
      <slot name="notice-board">
        <SakuraNoticeBoard />
      </slot>

      <slot name="post-pinned">
        <SakuraPinnedPost v-if="themeConfig.pinnedPost" />
      </slot>

      <slot name="post-list">
        <SakuraPostList />
      </slot>

      <template v-if="$slots.right" #right>
        <slot name="right" />
      </template>

      <template v-if="$slots.left" #left>
        <slot name="left" />
      </template>
    </SakuraMultiColumns>

    <slot name="pagination">
      <SakuraPagination />
    </slot>
  </div>
</template>

<style lang="scss">
@use 'valaxy/client/styles/mixins/index.scss' as *;

.sakura-home {
  // 首页无 hero 占位，给固定导航栏留出顶部空间，避免内容被遮挡
  padding-top: calc(var(--sakura-navbar-height, 64px) + 12px);
}

.sakura-home-layout {
  &.sakura-triple-columns {
    @include screen('md') {
      grid-template-columns: 0 1fr 0;
    }

    @include screen('lg') {
      grid-template-columns: 1fr 800px 1fr;
    }
  }

  &.sakura-two-columns-right {
    aside {
      display: none;
    }

    @include screen('md') {
      grid-template-columns: 1fr 0;
    }

    @include screen('lg') {
      grid-template-columns: 1fr 280px;

      aside {
        display: inline-block;
      }
    }

    @include mobile {
      gap: 0;
    }
  }

  &.sakura-one-columns {
    grid-template-columns: minmax(0, 800px);
  }
}
</style>
