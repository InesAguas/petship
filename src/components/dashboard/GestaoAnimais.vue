<template>
  <ModalInformacoesAnimal @close="modalInformacoesVisible = false" :animal="animalSelecionado" />
  <ModalAdicionarAnimal @close="modalAdicionarAnimalVisible = false" @novoAnimal="novoAnimal" @editarAnimal="editarAnimal" :animal="animalSelecionado"/>
  <!-- Modal para apagar um animal -->
  <div class="modal" tabindex="-1" id="modalApagar" @close="modalRemoverAnimalVisible = false">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{$t('dashboardAnimais.modalRemover')}}</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>  
        <div class="modal-body">
          {{$t('dashboardAnimais.modalRemoverTexto')}}
          {{ animalSelecionado.nome }}
          <p>{{$t('dashboardAnimais.modalRemoverTexto2')}}</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">{{$t('dashboardAnimais.modalRemoverCancelar')}}</button>
          <button type="button" class="btn text-white" data-bs-dismiss="modal" style="background-color:#FD7E14"
            @click="remover">{{$t('dashboardAnimais.modalRemoverConfirmar')}}</button>
        </div>
      </div>
    </div>
  </div>

  <NavBar></NavBar>
  <div class="container-fluid ms-0">
    <div class="row flex-nowrap">
      <PainelDashboard :isActive="'animais'"></PainelDashboard>
      <div class="col-8">
        <div class="row my-3">
          <div class="col">
            <h2 class="fw-bold" style="color: #653208;">{{$t('dashboardAnimais.titulo')}}</h2>
          </div>
          <div class="col">
            <button type="button" class="btn text-white fw-bold float-end" style="background-color: #FD7E14;"  @click="abrirModalAdicionar"
              data-bs-toggle="modal" data-bs-target="#formRegisto">{{$t('dashboardAnimais.botaoAdicionar')}}</button>
          </div>
        </div>

        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">{{$t('dashboardAnimais.nome')}}</th>
              <th scope="col">{{$t('dashboardAnimais.especie')}}</th>
              <th scope="col">{{$t('dashboardAnimais.sexo')}}</th>
              <th scope="col">{{$t('dashboardAnimais.idade')}}</th>
              <th scope="col">{{$t('dashboardAnimais.chip')}}</th>
              <th scope="col">{{$t('dashboardAnimais.anunciado')}}</th>
              <th scope="col">{{$t('dashboardAnimais.acoes')}}</th>
            </tr>
          </thead>
          <tbody v-if="animais">
            <tr v-for="animal in animais" :key="animal">
              <td style="width:10%;vertical-align: middle">
                <img v-if="animal.fotografia" :src="animal.fotografia" class="rounded-circle"
                  style="width:50px; height:50px; object-fit:cover"> {{ animal.nome }}
              </td>
              <td style="width:10%;vertical-align: middle">{{ animal.especie }}</td>
              <td style="width:10%;vertical-align: middle">{{ animal.sexo }}</td>
              <td style="width:10%;vertical-align: middle">{{ animal.idade }}</td>
              <td style="width:10%;vertical-align: middle">{{ animal.chip ? animal.chip : $t('dashboardAnimais.semChip') }}</td>
              <td style="width:10%;vertical-align: middle">
                <svg v-if="animal.anunciado" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="green"
                  class="bi bi-check-circle-fill" viewBox="0 0 16 16">
                  <path
                    d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zm-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z" />
                </svg>
                <div v-else>
                <svg  xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="Tomato"
                  class="bi bi-x-circle-fill" viewBox="0 0 16 16">
                  <path
                    d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293 5.354 4.646z" />
                </svg>
                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" fill="currentColor" class="bi bi-box-arrow-up-right ms-5" viewBox="0 0 16 16">
                  <path fill-rule="evenodd" d="M8.636 3.5a.5.5 0 0 0-.5-.5H1.5A1.5 1.5 0 0 0 0 4.5v10A1.5 1.5 0 0 0 1.5 16h10a1.5 1.5 0 0 0 1.5-1.5V7.864a.5.5 0 0 0-1 0V14.5a.5.5 0 0 1-.5.5h-10a.5.5 0 0 1-.5-.5v-10a.5.5 0 0 1 .5-.5h6.636a.5.5 0 0 0 .5-.5z"/>
                  <path fill-rule="evenodd" d="M16 .5a.5.5 0 0 0-.5-.5h-5a.5.5 0 0 0 0 1h3.793L6.146 9.146a.5.5 0 1 0 .708.708L15 1.707V5.5a.5.5 0 0 0 1 0v-5z"/>
                  <router-link :to="'/dashboard/anuncios?animalanuncio=' + animal.id" target="_blank"><rect class="btn" x="0" y="0" width="14" height="14" fill="transparent" /></router-link>
                </svg>
              </div>
              </td>
              <td style="width:10%;vertical-align: middle">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="orange"
                  class="bi bi-pencil-square me-3" viewBox="0 0 16 16">
                  <path
                    d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z" />
                  <path fill-rule="evenodd"
                    d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z" />
                  <rect class="btn" x="0" y="0" width="24" height="24" fill="transparent" @click="abrirEditar(animal)" data-bs-toggle="modal"
                  data-bs-target="#formRegisto" />

                </svg>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="red" class="bi bi-trash me-3"
                  viewBox="0 0 16 16">
                  <path
                    d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6Z" />
                  <path
                    d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1ZM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118ZM2.5 3h11V2h-11v1Z" />
                  <rect class="btn" x="0" y="0" width="24" height="24" fill="transparent"
                    @click="abrirModalRemover(animal)" data-bs-toggle="modal" data-bs-target="#modalApagar" />
                </svg>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="green" class="bi bi-eye"
                  viewBox="0 0 16 16">
                  <path
                    d="M16 8s-3-5.5-8-5.5S0 8 0 8s3 5.5 8 5.5S16 8 16 8zM1.173 8a13.133 13.133 0 0 1 1.66-2.043C4.12 4.668 5.88 3.5 8 3.5c2.12 0 3.879 1.168 5.168 2.457A13.133 13.133 0 0 1 14.828 8c-.058.087-.122.183-.195.288-.335.48-.83 1.12-1.465 1.755C11.879 11.332 10.119 12.5 8 12.5c-2.12 0-3.879-1.168-5.168-2.457A13.134 13.134 0 0 1 1.172 8z" />
                  <path d="M8 5.5a2.5 2.5 0 1 0 0 5 2.5 2.5 0 0 0 0-5zM4.5 8a3.5 3.5 0 1 1 7 0 3.5 3.5 0 0 1-7 0z" />
                  <rect class="btn" x="0" y="0" width="24" @click="abrirModalInformacoes(animal)" data-bs-toggle="modal"
                    data-bs-target="#modalInformativo" height="24" fill="transparent" />
                </svg>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
import NavBar from '../NavBar.vue';
import PainelDashboard from './PainelDashboard.vue';
import ModalInformacoesAnimal from './ModalInformacoesAnimal.vue';
import ModalAdicionarAnimal from './ModalAdicionarAnimal.vue';

export default {
  name: 'GestaAnimais',
  components: {
    NavBar,
    PainelDashboard,
    ModalInformacoesAnimal,
    ModalAdicionarAnimal
  },
  data() {
    return {
      animais: [],
      animalSelecionado: {
        nome: null,
        sexo: 1,
        especie: 1,
        raca: 1,
        idade: 1,
        cor: 1,
        porte: 1,
        data_recolha: null,
        local_captura: null,
        ferido: 0,
        agressivo: 0,
        identificacao: 0,
        chip: null,
        desparasitacao: null,
        temperatura: null,
        medicacao: null,
        fotografia: null
      },
      modalInformacoesVisible: false,
      modalRemoverAnimalVisible: false,
      modalAdicionarAnimalVisible: false,
    }
  },
  watch: {
        '$i18n.locale': function () {
            this.loadAnimais();
        }
    },
  mounted() {
    this.loadAnimais();
  },
  methods: {
    loadAnimais() {
      this.axios.get("associacao/animais", {
    }).then(response => {
      this.animais = response.data.animais
    })
    },
    abrirModalInformacoes(data) {
      this.animalSelecionado = data
      this.modalInformacoesVisible = true
    },

    abrirModalRemover(data) {
      this.animalSelecionado = data
      this.modalRemoverAnimalVisible = true
    },

    abrirEditar(data) {
      this.animalSelecionado = data
      this.modalAdicionarAnimalVisible = true
    },

    abrirModalAdicionar() {
      this.animalSelecionado = {
        nome: null,
        sexo: 1,
        especie: 1,
        raca: 1,
        idade: 1,
        cor: 1,
        porte: 1,
        data_recolha: null,
        local_captura: null,
        ferido: 0,
        agressivo: 0,
        identificacao: 0,
        chip: null,
        desparasitacao: null,
        temperatura: null,
        medicacao: null,
        fotografia: null
      }
      this.modalAdicionarAnimalVisible = true

    },

    remover() {
      this.axios.delete('removeranimal/' + this.animalSelecionado.id, {

      }).then(() => {
        let index = this.animais.indexOf(this.animalSelecionado)
        this.animais.splice(index, 1)
        this.modalRemoverAnimalVisible = false
        alert(this.$t('mensagens.animalRemovido'))
      })
        .catch(() => {
          alert(this.$t('mensagens.erro'))
        });
    },

    novoAnimal(data) {
      this.animais.unshift(data)
    },

    editarAnimal(data) {
      let index = this.animais.indexOf(this.animalSelecionado)
      this.animais.splice(index, 1, data)
    },


    
  }
}
</script>

<style>
.modal-backdrop {
  height:100%;
  width:100%;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>