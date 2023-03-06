<script setup>

import { reactive } from 'vue';

const tareas = reactive([
    {
        descripcion: 'Tarea de ejemplo',
        estatus: 'Pendiente',
    }
  ])

const estatuses = reactive(['Pendiente', 'En proceso', 'Finalizada'])

let nuevaTarea = "";

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
    <div class="datosActividad">
        <h2 class="titulo">Trabajo 05: To Do List con Vue JS</h2>
        <h4 class="datos">Miguel Ángel Manzano Méndez - Programación para Internet - I5909 - D03</h4>
    </div>
    <br>

    <div class="input">
        <input class="inputField" v-model="nuevaTarea" type="text" placeholder="Ingrese una nueva tarea">
        <button @click="crearTarea" class="botonCrear">CREAR</button>
    </div>

    <table class="table table-striped table-bordered">
        <thead class="headTabla">
            <tr class="columnasTabla">
                <th scope="col">Tarea</th>
                <th scope="col">Estatus</th>
                <th scope="col">Editar</th>
                <th scope="col">Eliminar</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="tarea, index in tareas" v-bind:key="index" class="filasTabla">
                <th><span :class="{'finalizadaEstilo': tarea.estatus == 'Finalizada' }" >{{ tarea.descripcion }} </span></th>
                <td><span class="estatus" @click="cambiarEstatus(index)" 
                    :class="{'pendiente': tarea.estatus == 'Pendiente',
                    'enProceso': tarea.estatus == 'En proceso',
                    'finished': tarea.estatus == 'Finalizada'

                }" title="Cambia el estatus">{{ tarea.estatus }}</span></td>
                <td>
                    <button class="botonEditar" @click="editarTarea(index)">Editar</button>
                </td>
                <td>
                    <button class="botonEliminar" @click="eliminarTarea(index)">Eliminar</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<style scoped>

.mainDiv{
    background-color: rgb(195, 219, 212);
}

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

.enProceso{
    color: rgb(224, 147, 4);
}

.datosActividad{
    text-align: center;
    margin-top: 50px;
}

.titulo{
    color: rgb(23, 128, 88);
}

.datos{
    margin-top: 20px;
}

.input{
    text-align: center;
    margin-bottom: 70px;
}

.inputField{
    width: 500px;
    margin-right: 5px;
}

.tarea{
    background-color: rgb(224, 224, 224);
}

.table{
    margin: auto;
    width: 90%;
    margin-bottom: 60px;
}

.headTabla{
    background-color: rgb(20, 20, 20);
}

.columnasTabla{
    text-align: center;
    font-size: 20px;
    color: rgb(36, 180, 125);
}

.filasTabla{
    text-align: center;
    font-weight: 500;
}

.botonCrear{
    background-color: rgb(34, 185, 148);
    font-weight: 700;
    border-radius: 5px;
}

.botonCrear:Hover{
    background-color: rgb(26, 156, 124);
}

.botonEditar{
    background-color: rgb(255, 180, 19);
    font-weight: 700;
    border-radius: 5px;
}

.botonEditar:Hover{
    background-color: rgb(231, 165, 21);
}

.botonEliminar{
    background-color: rgb(255, 39, 39);
    font-weight: 700;
    border-radius: 5px;
}

.botonEliminar:Hover{
    background-color: rgb(197, 32, 32);
}
</style>
