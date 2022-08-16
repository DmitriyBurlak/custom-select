<template>
    <div class="db_select">
        <select class="select" v-model="inputValue">
            <option v-for="(option, id) in data.options" :key="id" :value="data.isCheckObject ? option.value : option" :disabled="disabled">{{data.isCheckObject ? option.label : option}}</option>
        </select>

        <div class="custom-select" :class="{'focus': inputValue, 'active': optionShow}">
            <label class="label" for="cus-input">{{placeholder}}</label>
            <input type="text" id="cus-input" readonly="readonly" autocomplete="off" class="input" v-model="valueInput" @click="optionShow = !optionShow" :disabled="disabled">
            
            <div class="icon"></div>
            <span v-if="data.error" class="error">{{data.error}}</span>
            <div class="options" v-if="!data.error && data.options">
                <ul>
                    <template v-if="data.isCheckObject">
                        <li v-for="(item, idx) in data.options" :key="idx" @click="setInputValue(item.value)">  
                            {{item.label}}
                        </li>
                    </template>
                    <template v-else>
                        <li  v-for="(item, idx) in data.options" :key="idx" @click="setInputValue(item)">
                            {{item}}
                        </li>
                    </template>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            data: {},
            optionShow: false,
            inputValue: null,
        }
    },
    props: {
        modelValue: String,
        options: [Array, Object],
        placeholder: {type: String, default: null},
        disabled: {type: Boolean, default: false},
    },
    watch: {
        inputValue(newVal) {
            this.changeFn();
            this.selectOption(newVal);
        }
    },
    computed: {
        valueInput() {
            let value = "";
            if (this.data.isCheckObject) {
                this.data.options.forEach(item => item.value == this.inputValue ? value = item.label : null);
            }
            return this.data.isCheckObject ? value : this.inputValue;
        },
    },
    mounted() {
        this.dataColection();
        this.modelValue && (this.inputValue = this.modelValue);

        document.addEventListener("mousedown", e => {
            if (!e.target.closest(".custom-select")) {
                this.optionShow = false;
            }
        });
    },
    methods: {
        dataColection() {
            if (this.options) {
                let type = this.options.constructor.name;

                if (type == "Array") {
                    this.data.options = this.options;
                    this.data.isCheckObject = this.checkObject();
                } else if (type == "Object") {
                    this.data.options = Object.values(this.options);
                    this.data.isCheckObject = this.checkObject();
                    
                    //преобразование к значениям label value
                    if (this.data.isCheckObject) {
                        this.data.options = this.data.options.map(option => {
                            let values = Object.values(option);
                            return {label: values[0], value: values[1]}
                        })
                    }
                } else {
                    this.data.error = "Неверный формат";
                }
            } else {
                this.data.error = "Данные отсутсвуют";
            }
        },
        checkObject() {
            let arrType = this.data.options.map(i=>i.constructor.name); // собираю все типы
            let arr = arrType.filter((item, index) => arrType.indexOf(item) === index); //убираю дубликаты, если значение одно и оно Object

            if (arr.length == 1 && arr[0] == "Object" || arr.length == 1 && arr[0] == "String") {
                return arr.length == 1 && arr[0] == "Object";
            } else {
                this.data.error = "Неверный формат";
            }
        },
        setInputValue(value) {
            this.inputValue = value;
            this.optionShow = false;
        },
        changeFn() {
            this.$emit("change");
        },
        selectOption(value) {
            this.$emit("update:modelValue", value)
        },
    }
}
</script>

<style lang="scss" scoped>
*, *::before, *::after {
    box-sizing: border-box;
}
.select {
    // display: none;
}

.custom-select {
    width: max-content;
    position: relative;

    input {
        padding: 15px 60px 7px 15px;
        width: 100%;
        border: 1px solid #ccc;
        border-radius: 5px;
        outline: none;
        font-size: 15px;
        cursor: pointer;
    }

    .error {
        position: absolute;
        left: 0;
        bottom: -15px;
        width: 100%;
        font-size: 13px;
        color: #ff4646;
    }

    .icon {
        position: absolute;
        width: 20px;
        height: 20px;
        top: 50%;
        right: 0px;
        transform: translate(-50%, -50%);
        transition: all .3s linear;
        pointer-events: none;

        &:after {
            content: "";
            position: absolute;
            display: block;
            background-color: #d1d0d0;
            width: 2px;
            height: 10px;
            top: 50%;
            left: 5px;
            transform: translateY(-50%) rotate(45deg);
        }

        &:before {
            content: "";
            position: absolute;
            display: block;
            background-color: #d1d0d0;
            width: 10px;
            height: 2px;
            top: 50%;
            left: -5px;
            transform: translateY(-50%) rotate(45deg);
        }
    }
    .options {
        position: absolute;
        width: 100%;
        margin-top: 3px;
        z-index: 9999;
        transform-origin: center bottom;
        transform: scale3d(0,0,1);
        transition: transform .36s cubic-bezier(.4,0,.2,1);
        ul {
            padding: 0;
            margin: 0;
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid #ccc;
            li {
                list-style: none!important;
                background-color: #fff;
                padding: 8px 15px;
                cursor: pointer;

                &:hover {
                    background-color: #fbf2f2;
                }

            }

            &::-webkit-scrollbar-track {
                -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
                background-color: #F5F5F5;
            }

            &::-webkit-scrollbar {
                width: 8px;
                background-color: #F5F5F5;
            }

            &::-webkit-scrollbar-thumb {
                background-color: #5aa9f3;
            }
        }
    }
    .label {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        left: 16px;
        max-width: 90%;
        white-space: nowrap;
        overflow: hidden;
        pointer-events: none;
        color: #8e8e8e;
        transition: all .2s ease-out;
    }

    &.active {
        .input {
            border-color: #afafff;
        }
        .label {
            position: absolute;
            color: #afafff;
            top: 9px;
            font-size: 12px;
            left: 15px;
            
        }
        .icon {
            transform: translate(-50%, -50%) scale(1, -1);
        }
        .options {
            transform: scaleZ(1)
        }
    }

    &.focus {
        .label {
            position: absolute;
            color: #afafff;
            top: 9px;
            font-size: 12px;
            left: 15px;
        }
    }
}
</style>