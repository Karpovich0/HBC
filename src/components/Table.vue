<script>
import bookingsArr from '@/data/bookings.json';
import dateBlock from './dateBlock.vue';
import apartmentRow from './apartmentRow.vue';
export default {
    data() {
        return {};
    },
    mounted() {
        this.$store.dispatch('getWeek', 0);
        this.$store.dispatch('getApartmans', bookingsArr);
    },
    watch: {
        '$store.state.currentWeekPage'(value) {
            this.$store.dispatch('getWeek', value);
        },
    },
    methods: {
        filterBookings(apartmentName) {
            return bookingsArr.filter((item) => item.roomDetails.name == apartmentName);
        },
    },
    computed: {
        currentWeek() {
            return this.$store.state.week;
        },
        apartments() {
            return this.$store.state.apartments;
        },
    },
    components: {
        dateBlock: dateBlock,
        apartmentRow: apartmentRow,
    },
};
</script>
<template>
    <div class="table">
        <div class="table__head">
            <div class="buttons-wrapper">
                <button class="btn prev" @click="$store.state.currentWeekPage--">Prev</button>
                <button class="btn" @click="$store.state.currentWeekPage = 0">Today</button>
                <button class="btn next" @click="$store.state.currentWeekPage++">Next</button>
            </div>
            <div class="date-wrapper">
                <dateBlock v-for="(item, index) in currentWeek" :key="index" :date="item" />
            </div>
        </div>
        <div class="table__body">
            <apartmentRow
                v-for="(item, index) in apartments"
                :key="index"
                :apartmentName="item"
                :week="currentWeek"
                :bookings="filterBookings(item)"
            />
        </div>
    </div>
</template>
<style lang="scss">
.table {
    min-width: 800px;

    &__head {
        display: flex;
        align-items: stretch;
        .buttons-wrapper {
            flex-basis: 210px;
            max-width: 210px;
            padding: 10px;
            border-right: 1px solid var(--color-border);
            @media (max-width: 1024px) {
                flex-basis: 150px;
                max-width: 150px;
            }
            .btn {
                padding: 10px;
                background: transparent;
                border: 1px solid var(--color-border);
                cursor: pointer;
                color: var(--vt-c-white);
                font-size:15px;
                position: relative; 
                transition: 0.2s ease opacity;               
                &.prev {
                    padding-left: 20px;
                    border-bottom-left-radius: 20px;
                    border-top-left-radius: 20px;
                    background-color: rgb(217, 30, 24);
                }
                &.next {
                    padding-right: 20px;
                    border-bottom-right-radius: 20px;
                    border-top-right-radius: 20px;
                    background-color: rgb(45, 85, 255);
                }
                &:hover {
                    opacity: 0.8;
                }
                @media (max-width: 1024px) {
                    padding: 5px;
                }
            }
        }
        .date-wrapper {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            flex: 1;
            .date {
                text-align: center;
                border: 1px solid var(--color-border);
                padding: 10px;
                display: flex;
                color: var(--vt-c-white);
                align-items: center;
                justify-content: center;
                p {
                    font-weight: bold;
                    @media (max-width: 1024px) {
                        font-size: 13px;
                    }
                }
                @media (max-width: 1024px) {
                    padding: 5px;
                }
            }
        }
    }
    &__body {
        .apartment {
            display: flex;
            height: 75px;
            align-items: stretch;
            border: 1px solid var(--color-border);
            &__name {
                display:flex;
                align-items:center;
                flex-basis: 210px;
                max-width: 210px;
                padding: 10px 10px;
                color: var(--vt-c-white);
                border-right: 1px solid var(--color-border);
                @media (max-width: 1024px) {
                    padding: 20px 10px;
                    flex-basis: 150px;
                    max-width: 150px;
                    h3 {
                        font-size: 15px;
                    }
                }
            }
            &__status-on-date {
                flex: 1;
                display: grid;
                grid-template-columns: repeat(7, 1fr);
                .date {
                    border-right: 1px solid var(--color-border);                   
                }
            }
        }
    }
}
</style>
