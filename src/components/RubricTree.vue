<script setup>
import {ref, onMounted} from 'vue';
import axios from 'axios';
import RubricItem from '@/components/RubricItem.vue';

const rubrics = ref([]);
const allowEmpty = ref(false);
const totalSelectedCount = ref(0);

// ПО ХОРОШЕМУ В РЕАЛЬНЫХ ПРОЕКТАХ ЗАПРОСЫ НАДО ВЫНОСИТЬ В СЕРВИСЫ И BASE URL в .ENV ФАЙЛ,
// НО Т.К НАДО БЫСТРЕЕ ЗАКОНЧИТЬ Я ВЫНУЖДЕН СДЕЛАТЬ ТАК:

const baseUrl = 'https://www.klerk.ru/yindex.php/v3';
const getRubrics = async () => {
    const response = await axios.get(`${baseUrl}/event/rubrics?allowEmpty=${allowEmpty.value ? 1 : 0}`);
    rubrics.value = response.data;
};

const updateTotal = (count) => {
    totalSelectedCount.value += count;
};

onMounted(getRubrics);
</script>

<template>
    <div class="tree">
        <div class="tree__header">
            <h1 class="tree__title">Дерево рубрик</h1>
            <div class="tree__text">Сумма выбранных рубрик: <strong>{{ totalSelectedCount }}</strong></div>
        </div>
        <label class="tree__label">
            <input type="checkbox" v-model="allowEmpty" @change="getRubrics"/>
            Отображать пустые рубрики
        </label>
        <ul class="tree__list">
            <RubricItem
                v-for="rubric in rubrics"
                :key="rubric.id"
                :rubric="rubric"
                @update-total="updateTotal"
            />
        </ul>
    </div>
</template>

<style scoped lang="scss">
.tree {
    &__header {
        text-align: center;
    }

    &__title {
        font-size: 32px;
    }

    &__text {
        margin-top: 12px;
        font-size: 16px;
    }

    &__label {
        display: block;
        margin-top: 24px;
        margin-left: 16px;
        font-weight: bold;

        input {
            margin-right: 10px;
        }
    }

    &__list {
        width: 100%;
        margin-top: 16px;
    }
}
</style>
