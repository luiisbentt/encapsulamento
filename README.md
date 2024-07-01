package controller;


import model.Produto;
import model.Venda;
import view.ListaProdutosFrame;
import view.ProdutoFrame;
import view.VendaFrame;
import view.LojaFrame;


public class Controlador {
    // Instâncias das classes de modelo
    private Produto[] produtos;
    private Venda[] vendas;


    // Instâncias das classes de visualização
    private LojaFrame lojaFrame;
    private ProdutoFrame produtoFrame;
    private ListaProdutosFrame listaProdutosFrame;
    private VendaFrame vendaFrame;


    // Construtor
    public Controlador() {
        this.produtos = new Produto[0];
        this.vendas = new Venda[0];


        this.lojaFrame = new LojaFrame(this);
        this.produtoFrame = new ProdutoFrame(this);
        this.listaProdutosFrame = new ListaProdutosFrame(this);
        this.vendaFrame = new VendaFrame(this);
    }


    // Métodos do controlador
    public Produto[] estoque() {
        return produtos;
    }


    public void postQuantidade(int idProduto, int quantidade) {
        // Implementação para atualizar a quantidade de um produto
    }


    public void venderProduto(Venda venda) {
        // Implementação para registrar uma venda
    }


    public void cadastrarProduto(Produto produto) {
        // Implementação para cadastrar um novo produto
    }


    public Produto[] produtosCadastrados() {
        return produtos;
    }


    public Venda[] vendasRealizadas() {
        return vendas;
    }


    public void iniciarLoja() {
        // Método para iniciar a interface gráfica da loja
        lojaFrame.setVisible(true);
    }


    // Métodos para abrir outras interfaces gráficas
    public void abrirProdutoFrame() {
        produtoFrame.setVisible(true);
    }


    public void abrirListaProdutosFrame() {
        listaProdutosFrame.setVisible(true);
    }


    public void abrirVendaFrame() {
        vendaFrame.setVisible(true);
    }
}
```


### LojaFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import javax.swing.*;


public class LojaFrame extends JFrame {
    private JButton fechaCaixaButton;
    private JButton listaProdutosButton;
    private JButton novoProdutoButton;
    private JButton novaVendaButton;
    private Controlador controlador;


    public LojaFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Loja");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(null);


        fechaCaixaButton = new JButton("Fechar Caixa");
        fechaCaixaButton.setBounds(50, 50, 150, 30);
        add(fechaCaixaButton);


        listaProdutosButton = new JButton("Listar Produtos");
        listaProdutosButton.setBounds(50, 100, 150, 30);
        add(listaProdutosButton);


        novoProdutoButton = new JButton("Novo Produto");
        novoProdutoButton.setBounds(50, 150, 150, 30);
        add(novoProdutoButton);


        novaVendaButton = new JButton("Nova Venda");
        novaVendaButton.setBounds(50, 200, 150, 30);
        add(novaVendaButton);


        fechaCaixaButton.addActionListener(e -> System.exit(0));
        listaProdutosButton.addActionListener(e -> controlador.abrirListaProdutosFrame());
        novoProdutoButton.addActionListener(e -> controlador.abrirProdutoFrame());
        novaVendaButton.addActionListener(e -> controlador.abrirVendaFrame());
    }
}
```


### ProdutoFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import javax.swing.*;


public class ProdutoFrame extends JFrame {
    private JTextField nomeTextField;
    private JTextField precoTextField;
    private JButton salvarButton;
    private Controlador controlador;


    public ProdutoFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Cadastrar Produto");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        JLabel nomeLabel = new JLabel("Nome:");
        nomeLabel.setBounds(30, 30, 80, 25);
        add(nomeLabel);


        nomeTextField = new JTextField();
        nomeTextField.setBounds(100, 30, 150, 25);
        add(nomeTextField);


        JLabel precoLabel = new JLabel("Preço:");
        precoLabel.setBounds(30, 70, 80, 25);
        add(precoLabel);


        precoTextField = new JTextField();
        precoTextField.setBounds(100, 70, 150, 25);
        add(precoTextField);


        salvarButton = new JButton("Salvar");
        salvarButton.setBounds(100, 110, 150, 30);
        add(salvarButton);


        salvarButton.addActionListener(e -> {
            String nome = nomeTextField.getText();
            double preco = Double.parseDouble(precoTextField.getText());
            Produto produto = new Produto(nome, preco);
            controlador.cadastrarProduto(produto);
            setVisible(false);
        });
    }
}
```


### ListaProdutosFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import javax.swing.*;


public class ListaProdutosFrame extends JFrame {
    private JList<String> produtosList;
    private Controlador controlador;


    public ListaProdutosFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Lista de Produtos");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        produtosList = new JList<>();
        produtosList.setBounds(30, 30, 330, 200);
        add(produtosList);


        refreshList();
    }


    public void refreshList() {
        Produto[] produtos = controlador.produtosCadastrados();
        String[] nomesProdutos = new String[produtos.length];
        for (int i = 0; i < produtos.length; i++) {
            nomesProdutos[i] = produtos[i].getNome();
        }
        produtosList.setListData(nomesProdutos);
    }
}
```


### VendaFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import javax.swing.*;


public class VendaFrame extends JFrame {
    private JTextField totalTextField;
    private JButton okButton;
    private JButton cancelarButton;
    private Controlador controlador;


    public VendaFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Registrar Venda");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        JLabel totalLabel = new JLabel("Total:");
        totalLabel.setBounds(30, 30, 80, 25);
        add(totalLabel);


        totalTextField = new JTextField();
        totalTextField.setBounds(100, 30, 150, 25);
        add(totalTextField);


        okButton = new JButton("OK");
        okButton.setBounds(30, 70, 100, 30);
        add(okButton);


        cancelarButton = new JButton("Cancelar");
        cancelarButton.setBounds(150, 70, 100, 30);
        add(cancelarButton);


        okButton.addActionListener(e -> {
            double total = Double.parseDouble(totalTextField.getText());
            Venda venda = new Venda(0, new Produto[0], total); // Implementação para adicionar produtos vendidos
            controlador.venderProduto(venda);
            setVisible(false);
        });


        cancelarButton.addActionListener(e -> setVisible(false));
    }
}
```



package model;


public class Produto {
    private String nome;
    private double preco;
    private int quantidade;


    public Produto(String nome, double preco) {
        this.nome = nome;
        this.preco = preco;
        this.quantidade = 0;
    }


    public String getNome() {
        return nome;
    }


    public void setNome(String nome) {
        this.nome = nome;
    }


    public double getPreco() {
        return preco;
    }


    public void setPreco(double preco) {
        this.preco = preco;
    }


    public int getQuantidade() {
        return quantidade;
    }


    public void setQuantidade(int quantidade) {
        this.quantidade = quantidade;
    }


    @Override
    public String toString() {
        return "Produto{" +
                "nome='" + nome + '\'' +
                ", preco=" + preco +
                ", quantidade=" + quantidade +
                '}';
    }
}
```


#### ProdutoVendido.java (Pacote `model`)
```java
package model;


public class ProdutoVendido {
    private Produto produto;
    private int quantidadeVendida;
    private double precoTotal;


    public ProdutoVendido(Produto produto, int quantidadeVendida) {
        this.produto = produto;
        this.quantidadeVendida = quantidadeVendida;
        this.precoTotal = produto.getPreco() * quantidadeVendida;
    }


    public Produto getProduto() {
        return produto;
    }


    public void setProduto(Produto produto) {
        this.produto = produto;
    }


    public int getQuantidadeVendida() {
        return quantidadeVendida;
    }


    public void setQuantidadeVendida(int quantidadeVendida) {
        this.quantidadeVendida = quantidadeVendida;
    }


    public double getPrecoTotal() {
        return precoTotal;
    }


    public void setPrecoTotal(double precoTotal) {
        this.precoTotal = precoTotal;
    }


    @Override
    public String toString() {
        return "ProdutoVendido{" +
                "produto=" + produto +
                ", quantidadeVendida=" + quantidadeVendida +
                ", precoTotal=" + precoTotal +
                '}';
    }
}
```


#### Venda.java (Pacote `model`)
```java
package model;


import java.util.List;


public class Venda {
    private int id;
    private List<ProdutoVendido> produtosVendidos;
    private double total;


    public Venda(int id, List<ProdutoVendido> produtosVendidos, double total) {
        this.id = id;
        this.produtosVendidos = produtosVendidos;
        this.total = total;
    }


    public int getId() {
        return id;
    }


    public void setId(int id) {
        this.id = id;
    }


    public List<ProdutoVendido> getProdutosVendidos() {
        return produtosVendidos;
    }


    public void setProdutosVendidos(List<ProdutoVendido> produtosVendidos) {
        this.produtosVendidos = produtosVendidos;
    }


    public double getTotal() {
        return total;
    }


    public void setTotal(double total) {
        this.total = total;
    }


    @Override
    public String toString() {
        return "Venda{" +
                "id=" + id +
                ", produtosVendidos=" + produtosVendidos +
                ", total=" + total +
                '}';
    }
}
```


#### LVenda.java (Pacote `model`)
```java
package model;


import java.util.ArrayList;
import java.util.List;


public class LVenda {
    private List<Venda> vendas;


    public LVenda() {
        this.vendas = new ArrayList<>();
    }


    public void adicionarVenda(Venda venda) {
        vendas.add(venda);
    }


    public List<Venda> getVendas() {
        return vendas;
    }


    @Override
    public String toString() {
        return "LVenda{" +
                "vendas=" + vendas +
                '}';
    }
}


package view;


import controller.Controlador;
import javax.swing.*;


public class LojaFrame extends JFrame {
    private JButton fechaCaixaButton;
    private JButton listaProdutosButton;
    private JButton novoProdutoButton;
    private JButton novaVendaButton;
    private Controlador controlador;


    public LojaFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Loja");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(null);


        fechaCaixaButton = new JButton("Fechar Caixa");
        fechaCaixaButton.setBounds(50, 50, 150, 30);
        add(fechaCaixaButton);


        listaProdutosButton = new JButton("Listar Produtos");
        listaProdutosButton.setBounds(50, 100, 150, 30);
        add(listaProdutosButton);


        novoProdutoButton = new JButton("Novo Produto");
        novoProdutoButton.setBounds(50, 150, 150, 30);
        add(novoProdutoButton);


        novaVendaButton = new JButton("Nova Venda");
        novaVendaButton.setBounds(50, 200, 150, 30);
        add(novaVendaButton);


        fechaCaixaButton.addActionListener(e -> System.exit(0));
        listaProdutosButton.addActionListener(e -> controlador.abrirListaProdutosFrame());
        novoProdutoButton.addActionListener(e -> controlador.abrirProdutoFrame());
        novaVendaButton.addActionListener(e -> controlador.abrirVendaFrame());
    }
}
```


#### ProdutoFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import model.Produto;
import javax.swing.*;


public class ProdutoFrame extends JFrame {
    private JTextField nomeTextField;
    private JTextField precoTextField;
    private JButton salvarButton;
    private Controlador controlador;


    public ProdutoFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Cadastrar Produto");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        JLabel nomeLabel = new JLabel("Nome:");
        nomeLabel.setBounds(30, 30, 80, 25);
        add(nomeLabel);


        nomeTextField = new JTextField();
        nomeTextField.setBounds(100, 30, 150, 25);
        add(nomeTextField);


        JLabel precoLabel = new JLabel("Preço:");
        precoLabel.setBounds(30, 70, 80, 25);
        add(precoLabel);


        precoTextField = new JTextField();
        precoTextField.setBounds(100, 70, 150, 25);
        add(precoTextField);


        salvarButton = new JButton("Salvar");
        salvarButton.setBounds(100, 110, 150, 30);
        add(salvarButton);


        salvarButton.addActionListener(e -> {
            String nome = nomeTextField.getText();
            double preco = Double.parseDouble(precoTextField.getText());
            Produto produto = new Produto(nome, preco);
            controlador.cadastrarProduto(produto);
            setVisible(false);
        });
    }
}
```


#### ListaProdutosFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import model.Produto;
import javax.swing.*;


public class ListaProdutosFrame extends JFrame {
    private JList<String> produtosList;
    private Controlador controlador;


    public ListaProdutosFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Lista de Produtos");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        produtosList = new JList<>();
        produtosList.setBounds(30, 30, 330, 200);
        add(produtosList);


        refreshList();
    }


    public void refreshList() {
        Produto[] produtos = controlador.produtosCadastrados();
        String[] nomesProdutos = new String[produtos.length];
        for (int i = 0; i < produtos.length; i++) {
            nomesProdutos[i] = produtos[i].getNome();
        }
        produtosList.setListData(nomesProdutos);
    }
}
```


#### VendaFrame.java (Pacote `view`)
```java
package view;


import controller.Controlador;
import model.Venda;
import javax.swing.*;


public class VendaFrame extends JFrame {
    private JTextField totalTextField;
    private JButton okButton;
    private JButton cancelarButton;
    private Controlador controlador;


    public VendaFrame(Controlador controlador) {
        this.controlador = controlador;
        initialize();
    }


    private void initialize() {
        setTitle("Registrar Venda");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.HIDE_ON_CLOSE);
        setLayout(null);


        JLabel totalLabel = new JLabel("Total:");
        totalLabel.setBounds(30, 30, 80, 25);
        add(totalLabel);


        totalTextField = new JTextField();
        totalTextField.setBounds(100, 30, 150, 25);
        add(totalTextField);


        okButton = new JButton("OK");
        okButton.setBounds(30, 70, 100, 30);
        add(okButton);


        cancelarButton = new JButton("Cancelar");
        cancelarButton.setBounds(150, 70, 100, 30);
        add(cancelarButton);


        okButton.addActionListener(e -> {
            double total = Double.parseDouble(totalTextField.getText());
            Venda venda = new Venda(0, new Produto[0], total); // Implementação para adicionar produtos vendidos
            controlador.venderProduto(venda);
            setVisible(false);
        });


        cancelarButton.addActionListener(e -> setVisible(false));
    }
}
