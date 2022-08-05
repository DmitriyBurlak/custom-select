<template>
    <div>
        <select class="select" v-model="selectValue">
            <option v-for="(option, id) in options" :key="id" :value="checkObject ? option.value : option" :disabled="disabled">{{checkObject ? option.label : option}}</option>
        </select>


        <slot name="placeholder" class="label" v-if="$slots.placeholder"></slot>
        <div class="custom-select" :class="{'focus': inputValue, 'active': optionShow}">
            <label v-if="!$slots.placeholder" class="label" id="for">{{placeholder}}</label>
            <input type="text" readonly="readonly" autocomplete="off" class="input" v-model="inputValue" @click="optionShow = !optionShow" :disabled="disabled">
        
            <div class="icon"></div>
            <div class="options" v-if="options">
                <ul>
                    <template v-if="checkObject">
                        <li v-for="(item, idx) in options" :key="idx" @click="selectOption(item.label, item.value), changeFn()">
                            {{item.label}}
                        </li>
                    </template>
                    <template v-else>
                        <li  v-for="(item, idx) in options" :key="idx" @click="selectOption(item), changeFn()">
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
            optionShow: false,
            checkObject: false,
            inputValue: null,
            // selectValue: null
        }
    },
    computed: {
        selectValue() {
            let value = "";
            if (this.checkObject) {
                this.options.forEach(item => item.label == this.inputValue ? value = item.value : null);
            }
            return this.checkObject ? value : this.inputValue;
        }
    },
    props: {
        modelValue: String,
        options: Array,
        placeholder: {type: String, default: null},
        disabled: {type: Boolean, default: false},
    },
    mounted() {
        if(this.modelValue) {
            this.inputValue = this.modelValue
            let label = '';
            this.options.forEach(item => item.value == this.inputValue ? label = item.label : null)
            this.selectOption(label ? label : this.inputValue, this.inputValue)
        }

        document.addEventListener('mousedown', e => {
            if(!e.target.closest(".db_select")) {
                this.optionShow = false;
            }
        });

        let arrType = this.options.map(i=>i.constructor.name); // собираю все типы
        let arr = arrType.filter((item, index) => arrType.indexOf(item) === index); //убираю дубликаты, если значение одно и оно Object
        this.checkObject = arr.length == 1 && arr[0] == 'Object';
    },
    methods: {
        changeFn() {
            this.$emit('change');
        },
        selectOption(label, value) {
            if(!value) {
                this.$emit('update:modelValue', label)
            } else {
                this.$emit('update:modelValue', value)
            }
            this.optionShow = false;
            this.inputValue = label;
            // this.selectValue = this.checkObject ? value : label;
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
    margin-bottom: 10px;
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
            content: '';
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
            content: '';
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
        z-index: 999;
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
        .label {
            position: absolute;
            color: blue;
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
            color: blue;
            top: 9px;
            font-size: 12px;
            left: 15px;
        }
    }
}
</style>