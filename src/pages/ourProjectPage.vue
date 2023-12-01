<template>
    <div>
        <section class="articles m-b-200">
            <img src="@/assets/image/Our-project_photo.png" alt="interior photo" class="articles__img">
            <div class="articles__wrap">
                <h2 class="heading m-b-12">Our Project</h2>
                <div class="breadcrump">
                    <router-link to="/" class="breadcrump__item">Home</router-link>
                    <router-link to="/project" class="breadcrump__item">Project</router-link>
                </div>
            </div>
        </section>
        <div class="sort-area center m-b-61">

            <div class="sort-area__wrap">
                <div v-for="blockSortArea in blockSortAreas" :key="blockSortArea.id" class="sort-area__btn"
                    :class="{ 'sort-area__btn_active': selectedBlockSortArea === blockSortArea }"
                    @click="handleBlockSortAreaSelected(blockSortArea)">{{ blockSortArea
                    }}
                </div>
            </div>
        </div>
        <div class="project-card center m-b-61">
            <div class="project-card__wrap" :class="extraProjectCardClasses">
                <div v-for="card in displayedCards" :key="card.id" class="project-card__item">
                    <img :src="require(`@/assets/${card.img}`)" alt="interior" class="project-card__img">
                    <h3 class="project-card__subheading">{{ card.title }}</h3>
                    <p class="project-card__text">{{ card.text }}</p>
                    <router-link to="/project-details" class="project-card__btn">
                        <svg xmlns="http://www.w3.org/2000/svg" width="70" height="70" viewBox="0 0 70 70" fill="none">
                            <circle cx="35" cy="35" r="35" fill="#F4F0EC" />
                            <path d="M32 44L40 35L32 26" stroke="#292F36" stroke-width="2" stroke-linecap="round"
                                stroke-linejoin="round" />
                        </svg>
                    </router-link>
                    <!-- <img src="" alt="" class="project-card__like"> -->
                    <svg class="project-card__like" @click="handleLikeClick(card)" :class="{ 'liked': card.like }"
                        xmlns="http://www.w3.org/2000/svg" width="36" height="35" viewBox="0 0 36 35" fill="none">
                        <g filter="url(#filter0_d_993_156)">
                            <path
                                d="M16.0992 1.80951C16.7004 -0.0279021 19.2996 -0.0279021 19.9008 1.80951L21.7983 7.60879C22.0673 8.43084 22.8342 8.98685 23.6992 8.98685H29.8097C31.7506 8.98685 32.5539 11.4732 30.98 12.6088L26.0604 16.1581C25.3546 16.6674 25.0591 17.5748 25.3298 18.402L27.2146 24.1623C27.8166 26.0025 25.7137 27.539 24.1435 26.4062L19.1702 22.818C18.4715 22.3139 17.5285 22.3139 16.8298 22.818L11.8565 26.4062C10.2863 27.539 8.18335 26.0025 8.78545 24.1623L10.6702 18.402C10.9409 17.5748 10.6454 16.6674 9.93955 16.1581L5.02004 12.6088C3.44611 11.4732 4.24942 8.98685 6.19025 8.98685H12.3008C13.1658 8.98685 13.9327 8.43084 14.2017 7.60879L16.0992 1.80951Z"
                                fill="#FFA928" />
                        </g>
                        <defs>
                            <filter id="filter0_d_993_156" x="0.186424" y="0.431396" width="35.6271" height="34.3619"
                                filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
                                <feFlood flood-opacity="0" result="BackgroundImageFix" />
                                <feColorMatrix in="SourceAlpha" type="matrix"
                                    values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha" />
                                <feOffset dy="4" />
                                <feGaussianBlur stdDeviation="2" />
                                <feComposite in2="hardAlpha" operator="out" />
                                <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0" />
                                <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow_993_156" />
                                <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow_993_156" result="shape" />
                            </filter>
                        </defs>
                    </svg>
                </div>
            </div>

        </div>
        <paginationComponent class="center m-b-200" />

    </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex'

import paginationComponent from '../components/paginationComponent.vue'

export default {
    props: {
    },
    data() {
        return {
            pageKey: 'project',
            cardsPerPage: 4,
        }
    },
    components: {
        paginationComponent,
    },
    computed: {
        ...mapState(['selectedBlockSortArea', 'blockSortAreas', 'extraProjectCardClasses']),
        currentPage() {
            return this.$store.state.currentPage
            // Access currentPage directly
        },
        ...mapGetters(['displayedCards'])
    },
    created() {
        // Используйем Vuex для установки значения pageKey cardsPerPage
        this.setPageKey(this.pageKey)
        this.setCardsPerPage(this.cardsPerPage)
    },
    methods: {
        ...mapActions(['setSelectedBlockSortArea', 'setCurrentPage', 'setPageKey', 'setCardsPerPage']),
        handleBlockSortAreaSelected(blockSortArea) {
            this.setCurrentPage(1)
            if (blockSortArea === this.selectedBlockSortArea) {
                this.setSelectedBlockSortArea(null)
                // Удалить дополнительные классы, связанные с тегом:
                const newClasses = {
                    ...this.extraProjectCardClasses,
                    'project-card__wrap_bathroom': false,
                    'project-card__wrap_kitchen': false,
                    'project-card__wrap_bedroom': false,
                    'project-card__wrap_livingarea': false,
                }
                this.$store.commit('SET_ExtraProjectCardClass', newClasses)
            } else {
                this.setSelectedBlockSortArea(blockSortArea)

                const newClasses = {
                    ...this.extraProjectCardClasses,
                    'project-card__wrap_bathroom': blockSortArea === 'Bathroom',
                    'project-card__wrap_kitchen': blockSortArea === 'Kitchen',
                    'project-card__wrap_bedroom': blockSortArea === 'Bed Room',
                    'project-card__wrap_livingarea': blockSortArea === 'Living Area',
                }
                this.$store.commit('SET_ExtraProjectCardClass', newClasses)
            }
            this.$router.push(`/${this.pageKey}/${this.currentPage}`)
        },
        handleLikeClick(card) {
            card.like = !card.like
        }
    },
}
</script>

<style scoped lang="sass">
.articles
    position: relative
    &__img
        width: 100vw
        min-height: 24rem
    &__wrap
        border-radius: 3.7rem 3.7rem 0rem 0rem
        padding: 4.1rem 7.8rem
        text-align: center
        background: #FFF
        position: absolute
        bottom: 0
        left: 50%
        transform: translateX(-50%)
        @media (max-width: 767px)
            padding: 2.1rem 3.9rem

.breadcrump
    &__item
        color: #4D5053
        font-family: Jost, sans-serif
        font-size: 2.2rem
        font-style: normal
        font-weight: 400
        line-height: 3.3rem
        letter-spacing: 0.022rem
        text-decoration: none
        &:not(:last-child)::after
            padding-left: 8px
            padding-right: 8px
            content: '/'
        &_small
            font-size: 1.6rem
            line-height: 2.4rem
            letter-spacing: 0.016rem
            &:last-child
                padding-right: 7.8rem

.sort-area
    display: flex
    justify-content: center
    align-items: center
    // gap: 9.6rem
    
    color: #292F36
    text-align: center
    font-family: Jost, sans-serif
    font-size: 1.8rem
    font-style: normal
    font-weight: 600
    line-height: 2.25rem
    letter-spacing: 0.036rem
    &__wrap
        display: flex
        justify-content: space-between
        align-items: center
        border: 1px solid #CDA274
        border-radius: 1.8rem
        width: 88rem
        @media (max-width: 767px)
            flex-direction: column
            width: 90vw
    &__btn 
        padding: 2.6rem 6.6rem
        flex-shrink: 0
        border-radius: 2rem
        transition: all 0.4s ease-in-out
        &:hover, &_active 
            color: white
            background: #CDA274 

.project-card

    &__wrap
        display: flex
        flex-wrap: wrap
        flex-direction: column
        gap: 2rem
        max-height: 160rem
        @media (max-width: 767px)
            max-height: none
            flex-wrap: nowrap
        &_bathroom
            max-height: 130rem
            @media (max-width: 767px)
                max-height: none
        &_kitchen
            max-height: 130rem
            @media (max-width: 767px)
                max-height: none
        &_bedroom
            max-height: 160rem 
            @media (max-width: 767px)
                max-height: none
        &_livingarea
            max-height: 130rem
            @media (max-width: 767px)
                max-height: none
        
    &__item
        flex: 0 0 auto
        position: relative
        width: calc(50% - 2rem)
        height: fit-content
        position: relative
        @media (max-width: 767px)
            width: auto
    &__img
        width: 100%
        height: auto
        margin-bottom: 2.8rem
    &__subheading
        color: #292F36
        font-family: DM Serif Display, sans-serif
        font-size: 2.5rem
        font-style: normal
        font-weight: 400
        line-height: 3.125rem
        letter-spacing: 0.05rem
        margin-bottom: 0.4rem
    &__text
        color: #4D5053
        font-family: Jost, sans-serif
        font-size: 2.2rem
        font-style: normal
        font-weight: 400
        line-height: 3.3rem
        letter-spacing: 0.022rem
    &__btn
        display: flex
        width: 7rem
        height: 7rem
        border-radius: 50%
        border: none
        flex-shrink: 0
        position: absolute
        right: 0
        bottom: 0
        &:hover
            transform: scale(1.1)
        & svg
            width: 7rem
            height: 7rem
            @media (max-width: 767px)
                width: 6rem
                height: 6rem
    &__like
        position: absolute
        top: 0.5rem
        right: 0.5rem
        & path
            fill: #ffffff4f
        &.liked path
            fill: #FFA928

</style>