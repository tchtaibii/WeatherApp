<template>
    <div class="HourStatus">
        <div class="time">{{ hour }}:00</div>
        <img class="icon" :src="imgP" alt="Night">
        <div class="degresH">{{ tempC }}</div>
    </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
@Options({
    props: {
        hour: String,
        tempC: String,
        imgP: String,
    }
})
export default class WidgetContent extends Vue {
    data(): object {
        return {
        }
    }
}

</script>

<style scoped lang="scss">
@import '../assets/style.scss';

.HourStatus {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    gap: toRem(4);

    .time {
        font-family: 'Sarabun';
        font-style: normal;
        font-weight: 500;
        font-size: toRem(25);
        text-align: center;
        color: #FFFFFF;
    }

    .icon {
        width: 4rem;
        
    }

    .degresH {
        font-family: 'Sarabun';
        font-style: normal;
        font-weight: 500;
        font-size: toRem(25);
        text-align: center;
        color: #FFFFFF;
    }
}
</style>