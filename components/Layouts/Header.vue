<template lang="pug">
    header.header
        div.container
            button.header__toogle-menu(@click="menu_mobile = true")
                span.i-line
                span.i-line
                span.i-line

            a.header__logo(href="/" title="Blog da Apiki")
                img(:src="logo" alt="Blog da Apiki")

            SearchForm(device="desktop")

            nav.header__nav(v-if="menu")
                router-link(
                    v-for="link in menu"
                    :to="link.href"
                    v-bind:key="`desktop ${link.href}`"
                    class="header__nav-item"
                ) {{ link.text }}

            div.menu-nav-mobile(:class="menu_mobile ? '-opened' : ''")
                button.menu-nav-mobile__btn-close(@click="menu_mobile = false")
                    span

                .menu-nav-mobile__inner
                    .menu-header-container
                        ul.menu-nav-mobile__menu
                            li.menu-item(
                                v-for="link in menu"
                                v-bind:key="`mobile ${link.href}`"
                            )
                                router-link(:to="link.href") {{ link.text }}
                    
                    SearchForm(device="mobile")


</template>

<script>
import SearchForm from '@/components/SearchForm'

export default {
    name: 'Header',
    components: {
        SearchForm
    },
    data() {
        return {
            logo: require('@/assets/img/logo-apiki-blog.png'),
            menu_mobile: false,
        }
    },
    computed: {
        menu() {
            const menu = this.$store.state.pages.pages.map( item => {
                const menu_obj = {
                    href: `/page/${item.slug}`,
                    text: item.title.rendered
                }
                return menu_obj
            })
            .filter( item => item.text.indexOf(' ') < 0 )
            return menu
        }
    }
}
</script>