<template>
   <b-container>
    <h3 style="margin-left: 50px;">Seja Bem Vindo a Padaria da Nathy!</h3>
    <h4 style="margin-left: 80px;">Selecione o produto desejado</h4>
   <b-row>
    <b-table-simple striped hover>
      <b-thead>
      <b-th>Produto</b-th>
      <b-th>Preço</b-th>
      <b-th>Remover</b-th>
      <b-th>Quantidade</b-th>
      <b-th>Adicionar</b-th>
      </b-thead>
      <b-tbody>
        <b-tr v-for="item in items" :key="item.produto">
          <b-td>{{item.produto}}</b-td>
          <b-td>{{formatPrice(item.preco)}}</b-td>
          <b-td>
            <b-button size="sm" variant='danger' @click="removeProduct(item.produto)">-</b-button>
          </b-td>
          <b-td>{{item.quantidade}}</b-td>
          <b-td>
            <b-button variant='success' size="sm" @click="addProduct(item.produto)">+</b-button>
          </b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
    </b-row>
    <b-row class="justify-content-md-center">
      <h3>Valor total da compra: {{formatPrice(totalValue())}}</h3>
    </b-row>
    <b-row class="justify-content-md-center">
      <b-col cols="4" align-self="center">
        <b-input-group prepend="Forma de Pagamento">
         <b-form-select v-model="selected" :options="optionsFormaPagamento"></b-form-select>
        </b-input-group>
      </b-col>
      <b-col cols="3" v-show="selected=='dinheiro'">
        <b-input-group prepend="Pago R$">
          <b-form-input v-model="amountPaid"></b-form-input>
        </b-input-group>
      </b-col>
       <b-col cols="3" v-show="selected=='dinheiro'">
        <h3>Troco: {{formatPrice(formatValue() - totalValue())}}</h3>
      </b-col>
       <b-col cols="3" v-show="selected=='credito'">
        <b-input-group prepend="Tipo">
          <b-form-select v-model="cardPayment" :options="optionsCardPayment"></b-form-select>
        </b-input-group>
      </b-col>
      <b-col cols="2" v-show="selected != null">
          <b-button @click="payment(selected)" variant="success">Receber</b-button>
      </b-col>
    </b-row>
    <b-row  class="justify-content-md-center" style="margin-top: 20px;">
      <b-col cols="3" v-show="cardPayment=='online'">
           <b-input-group prepend="Nº Cartão">
          <b-form-input v-model="cardNumber"></b-form-input>
        </b-input-group>
      </b-col>
      <b-col cols="3" v-show="cardPayment=='online'">
           <b-input-group prepend="Validade">
          <b-form-input v-model="cardValidate"></b-form-input>
        </b-input-group>
      </b-col>
      <b-col cols="3" v-show="cardPayment=='online'">
           <b-input-group prepend="Código de Segurança">
          <b-form-input v-model="cardSecurityCode"></b-form-input>
        </b-input-group>
      </b-col>
    </b-row>
</b-container>
</template>

<script>
export default {
  name: "Padaria",
  props: {
    msg: String
  },
  data() {
    return {
      selected: null,
      cardPayment: null,
      amountPaid: '0,00',
      cardNumber: null,
      cardValidate: null,
      cardSecurityCode: null,
      items: [
        { produto: 'Pão Francês', preco: 0.25, quantidade: 0 },
        { produto: 'Pão Doce', preco: 0.50, quantidade: 0  },
        { produto: 'Leite', preco: 3.50, quantidade: 0  },
        { produto: 'Salgado', preco: 4.00, quantidade: 0  },
        { produto: 'Açucar', preco: 2.00, quantidade: 0  }
      ],
      optionsFormaPagamento: [
        { value: null, text: 'Selecione' },
        { value: 'dinheiro', text: 'Dinheiro' },
        { value: 'debito', text: 'Débito' },
        { value: 'credito', text: 'Crédito'},
      ],
      optionsCardPayment: [
        { value: null, text: 'Selecione' },
        { value: 'presencial', text: 'Presencial' },
        { value: 'online', text: 'On-Line' }
      ]
    }
  },
  methods: {
    addProduct(produto) {
      let itemPosicao = this.items.map(x => x.produto).indexOf(produto)
      this.items[itemPosicao].quantidade += 1;
      this.totalValue()
    },
    removeProduct(produto) {
      let itemPosicao = this.items.map(x => x.produto).indexOf(produto)
      if(this.items[itemPosicao].quantidade > 0) {
        this.items[itemPosicao].quantidade -= 1;
      }
      this.totalValue()
    },
    totalValue() {
      let total = this.items.reduce((sum, item) => {
        let valorProduto = item.preco*item.quantidade;
        return sum + valorProduto;
      },0)
      return total;
    },
    formatPrice(price) {
      return price.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    },
    formatValue() {
      let amount = this.amountPaid;
      return amount.replace(",", ".");
    },
    creditPayment() {
      if(this.cardPayment == "presencial") {
        alert("Obrigado por comprar conosco, volte sempre!");
      }
      else{
         if(!this.cardNumber || !this.cardValidate || !this.cardSecurityCode) {
          alert("Você precisa informar todos os dados do cartão!");
        }
      }
    },
    presentialPayment(type) {
      if(type == "dinheiro") {
        if(this.formatValue() - this.totalValue() > 0) {
          alert(`Obrigado por comprar conosco, seu troco é de ${this.formatPrice(this.formatValue() - this.totalValue())} Volte Sempre!`);
        }else{
          alert("O valor pago não é suficiente!")
        }
      }else{
        alert(`Obrigado por comprar conosco, Volte Sempre!`);
      }
    },
    payment(type) {
      if(type == "credito") {
        this.creditPayment();
      }else {
        this.presentialPayment(type);
      }
    }
  }
};
</script>