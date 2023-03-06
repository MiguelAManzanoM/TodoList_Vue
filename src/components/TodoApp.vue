<script setup>

import { reactive } from 'vue';

const tareas = reactive([
    {
        descripcion: 'Hacer la tarea',
        estatus: 'Pendiente',
    },
    {
        descripcion: 'Estudiar para el examen',
        estatus: 'En proceso',
    }
])

const estatuses = reactive(['Pendiente', 'En proceso', 'Finalizada'])

var nuevaTarea = '';

let tareaEditada = null;

function crearTarea(){
    if(nuevaTarea.length == 0){
        return
    }
    if(tareaEditada == null){
        tareas.push({
        descripcion: nuevaTarea,
        estatus: 'Pendiente',
        })
    }
    else{
        tareas[tareaEditada].descripcion = nuevaTarea;
        tareaEditada = null;
    }
    nuevaTarea = '';
}

function editarTarea(index){
    nuevaTarea = tareas[index].descripcion;
    tareaEditada = index;
}

function eliminarTarea(index){
    tareas.splice(index, 1)
}

function cambiarEstatus(index){
    let nuevoIndex = estatuses.indexOf(tareas[index].estatus);
    if(++nuevoIndex > 2){
        nuevoIndex = 0;
    }
    tareas[index].estatus = estatuses[nuevoIndex];
}

</script>

<template>
    <div class="main-container">
        <h2 class="titulo">To Do List con Vue JS</h2>
    </div>

    <div class="d-flex">
        <input v-model="nuevaTarea" type="text" class="form-control"/>
        <button @click="crearTarea" class="botonCrear">CREAR</button>
    </div>

    <table class="table table-bordered">
        <thead>
            <tr>
                <th scope="col">Tarea</th>
                <th scope="col">Estatus</th>
                <th scope="col">Editar</th>
                <th scope="col">Eliminar</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="tarea, index in tareas" v-bind:key="index">
                <th><span :class="{'finalizadaEstilo': tarea.estatus == 'Finalizada' }" >{{ tarea.descripcion }} </span></th>
                <td><span class="estatus" @click="cambiarEstatus(index)" 
                    :class="{'pendiente': tarea.estatus == 'Pendiente',
                    'progress': tarea.estatus == 'En proceso',
                    'finished': tarea.estatus == 'Finalizada'

                }">{{ tarea.estatus }}</span></td>
                <td>
                    <button @click="editarTarea(index)">Editar</button>
                </td>
                <td>
                    <button @click="eliminarTarea(index)">Eliminar</button>
                </td>
            </tr>
        </tbody>
    </table>

</template>

<style scoped>

.estatus{
    cursor: pointer;
}

.finalizadaEstilo{
    text-decoration: line-through;
}

.finished{
    color: green;
}

.pendiente{
    color: red;
}

.progress{
    color: gold;
}
</style>