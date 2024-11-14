<script setup>
import {ref} from 'vue';
import {defineProps, defineEmits} from 'vue';

const props = defineProps({
    rubric: Object,
});

const emit = defineEmits(['update-total']);
const isExpanded = ref(false);
const totalChildCount = ref(0);
const isSelected = ref(false);

const toggleChildren = () => {
    isExpanded.value = !isExpanded.value;
};

const toggleSelect = () => {
    const count = props.rubric.count + totalChildCount.value;

    emit('update-total', isSelected.value ? -count : count);
    isSelected.value = !isSelected.value;
};

const updateTotal = (count) => {
    totalChildCount.value += count;

    emit('update-total', count);
};
</script>

<template>
    <li class="rubric">
        <div class="rubric__wrapper">
            <input type="checkbox" class="rubric__checkbox" @change="toggleSelect"/>
            <a :href="`https://www.klerk.ru${rubric.url}`" target="_blank" class="rubric__link">
                {{ rubric.title }} - ({{ rubric.count }}, {{ totalChildCount }})
            </a>
            <button class="rubric__btn" :class="{'rubric__btn_active': isExpanded}" @click="toggleChildren">
                {{ isExpanded ? '˅' : '>' }}
            </button>
        </div>
        <ul v-show="isExpanded" class="rubric__list">
            <RubricItem
                v-for="child in rubric.children"
                :key="child.id"
                :rubric="child"
                @update-total="updateTotal"
            />
        </ul>
    </li>
</template>

<style scoped lang="scss">
.rubric {
    margin: 10px 0;
    padding: 8px 16px;
    background-color: #242424;
    border-radius: 5px;
    list-style: none;

    &__checkbox {
        margin-right: 10px;
    }

    &__link {
        color: var(--light-blue);
        text-decoration: none;

        &:hover {
            text-decoration: underline;
        }
    }

    &__btn {
        width: 28px;
        height: 28px;
        margin-left: 10px;
        background-color: #484848;
        color: #fff;
        font-size: 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: background-color 0.3s;

        &:hover {
            background-color: #575757;
        }

        &_active {
            background-color: var(--blue);

            &:hover {
                background-color: var(--blue);
            }
        }
    }
}
</style>
