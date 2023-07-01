<script setup>
    const props = defineProps(['tarefasPendentes'])
</script>
<template>
    <header class="p-5 mb-4 mt-4 bg-light rounded-3"> <!-- bg-light backgrond e rounded-3 para arredondar a borda -->
        <h1>Minhas tarefas</h1>
        <p>Você possui {{ props.tarefasPendentes }} tarefas pendentes</p>
    </header>
</template>