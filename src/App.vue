<template>
    <h1 class="title">{{title}}</h1>

    <div class="block">
        <Select :options="data" v-model="valueSelect" placeholder="Выберите значение">
            <!-- <template v-slot:placeholder>
                test
            </template> -->
        </Select>
        <span class="value">значение: {{valueSelect ? valueSelect : "Выберите значение"}}</span>
    </div>
    <div class="block">
        <Select :options="data2" v-model="valueSelect2" placeholder="Выберите значение" />
        <span class="value">значение: {{valueSelect2 ? valueSelect2 : "Выберите значение"}}</span>
    </div>
</template>

<script>
import Select from './components/Select/Select.vue'
export default {
    components: {Select},
    data() {
        return {
            title: "Custom select",
            data: ["Option1", "Option2", "Option3"],
            valueSelect: "",
            data2: [{label: "Options1", value: "1"}, {label: "Options2", value: "2"}, {label: "Options3", value: "3"}],
            valueSelect2: "2",
        }
    },
}
</script>

<style lang="scss">
    .title {
        text-align: center;
    }
    .block {
        display: flex;
        align-items: center;
        margin-bottom: 15px;

        .value {
            padding-left: 15px;
        }
    }
</style>