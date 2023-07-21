<template>
    <main>
        <div class="inicio">
            <div class="adicionar">
                <button @click="ativo = !ativo" id="adc">Adicionar tarefa</button>
            </div>
        </div>
        <div class="organizar">
            <div class="cont">
                <div class="container" v-for="tarefa in tarefas" :key="tarefa.id" :class="{ completed: tarefa.status }">
                <div class="tarefa">
                    <div class="titulo">
                        <h1>{{ tarefa.titulo }}</h1>
                    </div>
                    <div class="texto">
                        <p>{{ tarefa.descricao }}</p>
                    </div>
                    <div class="atual">
                        <div class="stt">
                            <p id="teste" v-if="tarefa.status == false">Pendente</p>
                            <p id="teste" v-if="tarefa.status == true">Concluído</p>
                        </div>
                    </div>
                </div>
                <div class="alterar">
                    <button @click="updateStatus(tarefa)" id="concluir">Marcar como concluído</button>
                    <button @click="limpar(tarefa)" id="excluir">Excluir tarefa</button>
                </div>
            </div>
            </div>

            <div class="nova_tarefa" v-if="ativo == true">
                <form>
                    <div class="nome">
                        <label for="titulo">Título da tarefa</label>
                        <input type="text" id="titulo">
                    </div>
                    <div class="desc">
                        <label for="descrição">Descrição</label>
                        <input type="text" id="descrição">
                    </div>
                    <div class="btn">
                        <button id="enviar" @click="adicionar(tarefa), ativo = !ativo">Adicionar tarefa</button>
                    </div>
                </form>
            </div>
        </div>
    </main>
</template>

<script>
export default {
    name: 'tarefas',
    data() {
        return {
            tarefas: [],
            ativo: false
        }
    },
    methods: {
        async loadbase() {
            const dados = await fetch("http://localhost:3000/dados");
            const base = await dados.json();
            this.tarefas = base;
        },
        async limpar(tarefa) {
            await fetch(`http://localhost:3000/dados/${tarefa.id}`, {
                method: 'DELETE'
            });
            location.reload();
        },

        async updateStatus(tarefa) {
            // Atualiza o status da tarefa no banco de dados
            await fetch(`http://localhost:3000/dados/${tarefa.id}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    ...tarefa,
                    status: !tarefa.status
                })
            });
            // Atualiza o status da tarefa localmente
            tarefa.status = !tarefa.status;
        },

        async adicionar(tarefa){
            let nome = document.getElementById('titulo').value;
            let desc = document.getElementById('descrição').value;

            await fetch ('http://localhost:3000/dados', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    ...tarefa,
                    titulo: nome,
                    descricao: desc,
                    status: false
                })
            })
            console.log("Nova tarefa adicionada")
            location.reload();
        }

    },
    mounted() {
        this.loadbase();
    }
}

</script>
<style scoped>
.container {
    background-color: #5b7389;
    border: black 1px solid;
    padding: 10px;
    margin: 5px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
    display: flex;
    width: 100%;
    border-radius: 5px;
}
.cont{
    width: 50%;
    justify-content: space-around;
    margin-right: 10%;
}
.container.completed {
    background-color: rgb(13, 255, 0);
}
.tarefa {
    width: 80%;
}
.alterar {
    display: flex;
    justify-content: center;
    align-items: center;
}
.alterar button {
    float: right;
}
.organizar{
    display: flex;
}
.nova_tarefa{
    
    width: 400px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
}
form{
    background-color: #2c3e50;
    height: auto;
    padding: 25px;
    border-radius: 10px;
}
#titulo,
#descrição{
    width: 90%;
    height: 25px;
    margin-left: 4%;
    border-radius: 5px;
    border: none;
    background-color: #5b7389;
    padding-left: 5px;
    margin-top: 8px;
}
label{
    margin-left: 4%;
    color: aliceblue;
}
.btn{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 30px;
}
#enviar{
    border: none;
    height: 30px;
    width: 200px;
    border-radius: 5px;
    background-color: rgb(13, 255, 0);
    cursor: pointer;
}
.nome{
   padding-top: 20px;
}
.desc{
    margin-top: 30px;
}
.adicionar{
    padding: 10px;
}
#concluir{
    margin-right: 5px;
    border: none;
    border-radius: 3px;
    padding: 3px;
    cursor: pointer;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    background-color: rgb(14, 188, 26);
}
#excluir{
    border: none;
    border-radius: 3px;
    padding: 3px;
    cursor: pointer;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    background-color: #ff1e1e;
}
#adc{
    border: none;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    background-color: #2c3e50;
    color: aliceblue;
    font-family: Avenir, Helvetica, Arial, sans-serif;
}
.titulo{
    color: aliceblue;
}
.texto{
    margin-top: 10px;
    color: aliceblue;
}
.atual{
    margin-top: 5px;
    color: black;
}
</style>

