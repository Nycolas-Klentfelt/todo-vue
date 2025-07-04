<script setup>
    import { reactive } from 'vue';
    import cabecalho from './components/cabecalho.vue';
    import formulario from './components/formulario.vue';
    import listaDeTarefas from './components/listaDeTarefas.vue';
    
    const estado = reactive({
        filtro: 'todas',
        tarefaTemp: '',
        tarefas: [
            {
                titulo: 'Estudar ES6',
                finalizada: false,
            },
            {
                titulo: 'Estudar SASS',
                finalizada: false,
            },
            {
                titulo: 'Ir para a academia',
                finalizada: true,
            }
        ]
    });

    const getTarefasPendentes = () => {
        return estado.tarefas.filter(tarefas => !tarefas.finalizada);
    };

    const getTarefasFinalizadas = () => {
        return estado.tarefas.filter(tarefas => tarefas.finalizada);
    };

    const getTarefasFiltradas = () => {
        const {filtro} = estado;

        switch (filtro) {
            case 'pendentes':
                return getTarefasPendentes();
            case 'finalizadas': 
                return getTarefasFinalizadas();
            default:
                return estado.tarefas
        };
    };

    const cadastraTarefa = () => {
        const tarefaNova = {
            titulo : estado.tarefaTemp,
            finalizada: false,
        }
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = '';
    }
</script>

<template>
    <div class="container">
        <cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
        <formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
        <listaDeTarefas :tarefas="getTarefasFiltradas()"/>
    </div>
</template>


