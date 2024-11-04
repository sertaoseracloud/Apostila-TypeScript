### Criando uma Pokedex com Vue.js e Bootstrap Grid

Neste artigo, vamos construir uma Pokedex simples usando Vue.js e o Bootstrap Grid para layout. A aplicação permitirá que os usuários visualizem uma lista de Pokémon, exibindo detalhes como nome, imagem e tipo. Vamos aproveitar a API pública do PokéAPI para buscar dados dos Pokémon.

#### Estrutura do Projeto

1. **HTML (Template)**: Usaremos o Bootstrap Grid para estruturar a apresentação dos Pokémon.
2. **JavaScript (Script)**: A lógica para buscar e gerenciar os dados dos Pokémon será implementada no Vue.js.
3. **CSS (Style)**: Estilizações básicas serão feitas usando Bootstrap para um design responsivo.

### Código Completo

#### 1. Configurando o Projeto

Primeiro, crie um novo projeto Vue.js. Você pode usar Vue CLI ou configurar manualmente. Para este exemplo, vamos assumir que você já tem o ambiente Vue configurado.

Instale o Bootstrap:

```bash
npm install bootstrap
```

Adicione o Bootstrap ao seu projeto no arquivo `main.js`:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

#### 2. Template

Aqui está o template principal da Pokedex. Usaremos o Bootstrap Grid para organizar os Pokémon em cards.

```html
<template>
  <div id="app" class="container mt-5">
    <h1 class="text-center mb-4">Pokedex</h1>
    <div class="row">
      <div class="col-md-4" v-for="pokemon in pokemons" :key="pokemon.id">
        <div class="card mb-4">
          <img :src="pokemon.sprites.front_default" class="card-img-top" alt="Pokemon Image" />
          <div class="card-body">
            <h5 class="card-title">{{ pokemon.name }}</h5>
            <p class="card-text">Tipo: {{ pokemon.types.map(type => type.type.name).join(', ') }}</p>
            <button @click="showDetails(pokemon)" class="btn btn-primary">Detalhes</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="selectedPokemon" class="modal fade show" style="display: block;" tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ selectedPokemon.name }}</h5>
            <button type="button" class="close" @click="closeDetails" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <img :src="selectedPokemon.sprites.front_default" class="img-fluid" />
            <p><strong>Tipos:</strong> {{ selectedPokemon.types.map(type => type.type.name).join(', ') }}</p>
            <p><strong>Altura:</strong> {{ selectedPokemon.height }}</p>
            <p><strong>Peso:</strong> {{ selectedPokemon.weight }}</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="closeDetails">Fechar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

#### 3. Script

Vamos implementar a lógica para buscar os dados dos Pokémon e gerenciar a seleção de detalhes.

```javascript
<script>
export default {
  data() {
    return {
      pokemons: [],
      selectedPokemon: null,
    };
  },
  created() {
    this.fetchPokemons();
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=20');
        const data = await response.json();
        this.pokemons = await Promise.all(data.results.map(async (pokemon) => {
          const details = await fetch(pokemon.url);
          return await details.json();
        }));
      } catch (error) {
        console.error('Error fetching Pokémon:', error);
      }
    },
    showDetails(pokemon) {
      this.selectedPokemon = pokemon;
    },
    closeDetails() {
      this.selectedPokemon = null;
    },
  },
};
</script>
```

#### 4. Estilos

Utilizaremos o Bootstrap para a maior parte da estilização, mas você pode adicionar estilos personalizados se necessário. O Bootstrap já proporciona um layout responsivo, portanto não há necessidade de CSS adicional para a estrutura básica.

### Explicação das Funcionalidades

- **Busca de Pokémon**: Ao inicializar a aplicação, a função `fetchPokemons` é chamada para buscar dados dos Pokémon usando a PokéAPI.
- **Exibição de Pokémon**: Cada Pokémon é exibido em um card utilizando o grid do Bootstrap. Cada card inclui uma imagem, nome e tipos.
- **Modal de Detalhes**: Ao clicar no botão "Detalhes", um modal é exibido com informações adicionais sobre o Pokémon selecionado.

### Conclusão

Neste artigo, construímos uma Pokedex simples usando Vue.js e Bootstrap. A estrutura modular e a abordagem reativa do Vue.js tornam a construção de interfaces interativas fácil e eficiente. Com Bootstrap, garantimos que a aplicação seja responsiva e atraente. Você pode expandir essa aplicação, adicionando funcionalidades como busca, filtragem ou uma visualização de detalhes mais rica para cada Pokémon.