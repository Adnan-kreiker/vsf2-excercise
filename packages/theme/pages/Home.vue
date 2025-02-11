<template>
  <div id="home">
    <SfHero class="hero">
      <SfHeroItem
        v-for="(hero, i) in heroes"
        :key="i"
        :title="hero.title"
        :subtitle="hero.subtitle"
        :button-text="hero.buttonText"
        :background="hero.background"
        :image="hero.image"
        :class="hero.className"
      />
      <template #prev> <span></span> </template>
      <template #next> <span></span> </template>
    </SfHero>
    <BestSellers></BestSellers>
    <ShoppingFor></ShoppingFor>
    <LazyHydrate when-visible>
      <RelatedProducts
        :products="products"
        :loading="productsLoading"
        title="Match it with"
      />
    </LazyHydrate>

    <LazyHydrate when-visible>
      <SfCallToAction
        title="BECOME A MEMBER"
        button-text="SIGN UP FOR FREE"
        description="Get 50% off just by registering"
        image="/homepage/cta.jpg"
        class="call-to-action"
      />
    </LazyHydrate>
    <LazyHydrate when-visible>
      <Trending />
    </LazyHydrate>
  </div>
</template>
<script type="module">
import { SfHero, SfCallToAction } from '@storefront-ui/vue';
import { useProduct, useCart, productGetters } from '@vue-storefront/shopify';
import { computed } from '@nuxtjs/composition-api';
import { onSSR } from '@vue-storefront/core';
import LazyHydrate from 'vue-lazy-hydration';
import MobileStoreBanner from '~/components/MobileStoreBanner.vue';
import RelatedProducts from '~/components/RelatedProducts.vue';
import ShoppingFor from '~/components/ShoppingFor.vue';
import BestSellers from '~/components/BestSellers.vue';
import Trending from '~/components/Trending.vue';

export default {
  name: 'Home',
  components: {
    BestSellers,
    Trending,
    SfHero,
    RelatedProducts,
    // SfBanner,
    SfCallToAction,
    ShoppingFor,
    // SfBannerGrid,
    MobileStoreBanner,
    LazyHydrate,
  },
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  setup() {
    const {
      products: relatedProducts,
      search: productsSearch,
      loading: productsLoading,
    } = useProduct('products');
    const { cart, load: loadCart, addItem: addToCart, isInCart } = useCart();

    onSSR(async () => {
      await productsSearch({ limit: 10, collections: 'accessories' });
      await loadCart();
    });
    return {
      products: computed(() =>
        productGetters.getFiltered(relatedProducts.value, { master: true })
      ),
      getChkId: computed(() => cart.value.id),
      productsLoading,
      productGetters,
      addToCart,
      isInCart,
    };
  },
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  data() {
    return {
      heroes: [
        {
          title: 'Colorful winter dresses are already in store',
          subtitle: 'winter COLLECTION 2022',
          buttonText: 'Learn more',
          background: '#eceff1',
          image: {
            mobile: '/homepage/banners/b1-m.jpg',
            desktop: '/homepage/banners/b1.jpg',
          },
          link: '/c/women/women-clothing-shirts',
        },
        {
          title: 'Colorful winter dresses are already in store',
          subtitle: 'winter COLLECTION 2022',
          buttonText: 'Learn more',
          background: '#fce4ec',
          image: {
            mobile: '/homepage/banners/b2-m.jpg',
            desktop: '/homepage/banners/b2.jpg',
          },
          link: '/c/women/women-clothing-dresses',
        },
        {
          title: 'Colorful winter dresses are already in store',
          subtitle: 'winter COLLECTION 2022',
          buttonText: 'Learn more',
          background: '#efebe9',
          image: {
            mobile: '/homepage/banners/b3-m.jpg',
            desktop: '/homepage/banners/b3.jpg',
          },
          link: '/c/women/women-shoes-sandals',
          className:
            'sf-hero-item--position-bg-top-left sf-hero-item--align-right',
        },
      ],
      banners: [
        {
          slot: 'banner-A',
          subtitle: 'Dresses',
          title: 'Cocktail & Party',
          description:
            "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands.",
          buttonText: 'Shop now',
          image: {
            mobile:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerB_328x343.jpg',
            desktop:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerF_332x840.jpg',
          },
          class: 'sf-banner--slim desktop-only',
          link: '/c/women/women-clothing-skirts',
        },
        {
          slot: 'banner-B',
          subtitle: 'Dresses',
          title: 'Linen Dresses',
          description:
            "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands.",
          buttonText: 'Shop now',
          image: {
            mobile:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerE_328x343.jpg',
            desktop:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerE_496x840.jpg',
          },
          class: 'sf-banner--slim banner-central desktop-only',
          link: '/c/women/women-clothing-dresses',
        },
        {
          slot: 'banner-C',
          subtitle: 'T-Shirts',
          title: 'The Office Life',
          image: {
            mobile:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerC_328x343.jpg',
            desktop:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerC_332x400.jpg',
          },
          class: 'sf-banner--slim banner__tshirt',
          link: '/c/women/women-clothing-shirts',
        },
        {
          slot: 'banner-D',
          subtitle: 'winter Sandals',
          title: 'Eco Sandals',
          image: {
            mobile:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerG_328x343.jpg',
            desktop:
              'https://cdn.shopify.com/s/files/1/0407/1902/4288/files/bannerG_332x400.jpg',
          },
          class: 'sf-banner--slim',
          link: '/c/women/women-shoes-sandals',
        },
      ],
    };
  },
  methods: {
    // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
    toggleWishlist(index) {
      this.products[index].isInWishlist = !this.products[index].isInWishlist;
    },
  },
};
</script>

<style lang="postcss" scoped>
.article-meta h4 a {
  color: #111111;
  font-weight: 600;
  font-size: 20px;
}
.article-meta {
  margin-top: 10px;
}
.article-item__meta-item:not(:last-child)::after {
  display: inline-block;
  content: '';
  width: 5px;
  height: 5px;
  margin: -1px 10px 0 10px;
  border-radius: 100%;
  background: rgba(0, 0, 0, 0.4);
  vertical-align: middle;
}
#home {
  box-sizing: border-box;
  padding: 0;
  width: 100%;
  @include for-desktop {
    /* max-width: 1440px; */
    padding: 0;
    margin: 0 auto;
    width: 100%;
  }
}
::v-deep .glide__track {
  min-height: 350px !important;
  max-height: calc(100vh - 111px) !important;
}
.hero {
  /* max-height: 545px; */

  margin: var(--spacer-xs) auto var(--spacer-lg);
  --hero-item-background-position: center;
  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }
  @include for-desktop {
    margin: var(--spacer-sm) auto var(--spacer-2xl);
  }
  .sf-hero-item {
    &:nth-child(even) {
      --hero-item-background-position: left;
      @include for-mobile {
        --hero-item-background-position: 30%;
        --hero-item-wrapper-text-align: right;
        --hero-item-subtitle-width: 100%;
        --hero-item-title-width: 100%;
        --hero-item-wrapper-padding: var(--spacer-sm) var(--spacer-sm)
          var(--spacer-sm) var(--spacer-2xl);
      }
    }
  }
}

::v-deep .sf-hero__controls {
  --hero-controls-display: none;
}

::v-deep .sf-hero-item {
  min-height: 350px;
}

.banner-grid {
  --banner-container-width: 50%;
  margin: var(--spacer-xl) 0;
  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }
  @include for-desktop {
    margin: var(--spacer-2xl) 0;
    ::v-deep .sf-link {
      --button-width: auto;
    }
  }
}

.banner {
  &__tshirt {
    background-position: left;
  }
  &-central {
    @include for-desktop {
      --banner-container-flex: 0 0 70%;
    }
  }
}

.similar-products {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: var(--spacer-2xs);
  --heading-padding: 0;
  border-bottom: 1px var(--c-light) solid;
  @include for-desktop {
    border-bottom: 0;
    justify-content: center;
    padding-bottom: 0;
  }
}

.call-to-action {
  background-position: right;
  margin: var(--spacer-xs) 0;
  @include for-desktop {
    margin: var(--spacer-xl) 0 var(--spacer-2xl) 0;
  }
}

.carousel {
  margin: 0 auto;
  @include for-desktop {
    margin: 0 auto;
  }
  &__item {
    margin: 1.375rem 0 2.5rem 0;
    @include for-desktop {
      margin: var(--spacer-xl) 0 var(--spacer-xl) 0;
    }
    &__product {
      --product-card-add-button-transform: translate3d(0, 30%, 0);
    }
  }
}
</style>
