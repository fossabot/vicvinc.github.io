<template>
  <header class="navbar">
    <div class="container">
      <SidebarButton @toggle-sidebar="$emit('toggle-sidebar')"/>
      <router-link :to="$localePath" class="home-link">
        <img class="logo" v-if="$site.themeConfig.logo" :src="$withBase($site.themeConfig.logo)">
        <span
          class="site-name"
          v-if="$siteTitle"
          :class="{ 'can-hide': $site.themeConfig.logo }"
        >{{ $siteTitle }}</span>
      </router-link>
      <div class="links">
        <AlgoliaSearchBox v-if="isAlgoliaSearch" :options="algolia"/>
        <SearchBox v-else-if="$site.themeConfig.search !== false"/>
        <NavLinks class="can-hide"/>
      </div>
    </div>
  </header>
</template>

<script>
import SidebarButton from "./SidebarButton.vue";
import AlgoliaSearchBox from "@AlgoliaSearchBox";
import SearchBox from "./SearchBox.vue";
import NavLinks from "./NavLinks.vue";

export default {
  components: { SidebarButton, NavLinks, SearchBox, AlgoliaSearchBox },
  computed: {
    algolia() {
      return (
        this.$themeLocaleConfig.algolia || this.$site.themeConfig.algolia || {}
      );
    },
    isAlgoliaSearch() {
      return this.algolia && this.algolia.apiKey && this.algolia.indexName;
    }
  }
};
</script>

<style lang="stylus">
@import './styles/config.styl';

.navbar {
  padding: 0.7rem 1.5rem;

  // line-height: $navbarHeight - 1.4rem;
  .container {
    position: relative;
  }

  a, span, img {
    display: inline-block;
  }

  .logo {
    height: $navbarHeight - 1.4rem;
    min-width: $navbarHeight - 1.4rem;
    margin-right: 0.8rem;
    vertical-align: top;
  }

  .site-name {
    font-size: 1.3rem;
    font-weight: 600;
    color: $textColor;
    position: relative;
  }

  .links {
    font-size: 1rem;
    position: absolute;
    right: 15px;
    top: 50%;
    transform: translateY(-50%);
  }
}

@media (max-width: $MQMobile) {
  .navbar {
    .container {
      padding-left: 4rem;
    }

    .can-hide {
      display: none;
    }
  }
}
</style>
