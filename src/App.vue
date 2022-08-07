<template>
    <h1 class="title">{{title}}</h1>

    <div class="info">Входные данные масив - {{data}}</div>
    <div class="block">
        <Select :options="data" v-model="valueSelect" placeholder="Выберите значение">
            <template v-slot:placeholder>
                <div>Выберите значение</div>
            </template>
        </Select>
        <span class="value">значение: {{valueSelect ? valueSelect : "Выберите значение"}}</span>
    </div>

    <div class="info">Входные данные масив - {{data2}}</div>
    <div class="block">
        <Select :options="data2" v-model="valueSelect2" placeholder="Выберите значение" />
        <span class="value">значение: {{valueSelect2 ? valueSelect2 : "Выберите значение"}}</span>
    </div>

    <div class="info">Входные данные масив - {{data3}}</div>
    <div class="block">
        <Select :options="data3" v-model="valueSelect3" placeholder="Выберите значение"></Select>
        <span class="value">значение: {{valueSelect3 ? valueSelect3 : "Выберите значение"}}</span>
    </div>

    <div class="info">Входные данные масив - {{data4}}</div>
    <div class="block">
        <Select :options="data4" v-model="valueSelect4" placeholder="Выберите значение"></Select>
        <span class="value">значение: {{valueSelect4 ? valueSelect4 : "Выберите значение"}}</span>
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
            data3: {value1: "Option1", value2: "Option2"},
            valueSelect3: "",
            data4: { value1: { title: "Option 1", id: "1" }, value2: { title: "Option 2", id: "2" } },
            valueSelect4: "",
        }
    },
}
</script>

<style lang="scss">
    .title {
        text-align: center;
    }
    .info {
        padding-top: 10px;
        color: #9f9393;
        font-style: italic;
    }
    .block {
        display: flex;
        align-items: flex-end;
        padding-bottom: 20px;
        padding: 20px 0;
        border-bottom: 1px solid #ccc;

        .value {
            padding-left: 15px;
        }
    }
</style>