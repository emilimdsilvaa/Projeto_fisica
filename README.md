import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

public class LancamentoVertical implements ActionListener {

    // Componentes da interface
    JFrame frame;
    JButton calcularBtn, resetarBtn;
    JTextField velocidadeInput, gravidadeInput;
    JTextField tempoSubidaOutput, alturaMaxOutput, tempoTotalOutput;

    // Variáveis para armazenar os valores digitados
    Double velocidadeInicial, gravidade;

    // Descrições para as saídas
    String tempoSubidaDescricao, alturaMaxDescricao, tempoTotalDescricao;

    // Construtor
    LancamentoVertical() {
        // Cria a janela
        frame = new JFrame("Simulador de Lançamento Vertical");

        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500, 500);
        frame.setLayout(null);

        // Criação dos labels e inputs
        JLabel velocidadeLabel = criarLabel("Velocidade inicial (m/s):", 30, 30, 180, 30);
        velocidadeInput = criarTextField("", 220, 30, 100, 30, true, true);

        JLabel gravidadeLabel = criarLabel("Gravidade (m/s²):", 30, 70, 180, 30);
        gravidadeInput = criarTextField("9.8", 220, 70, 100, 30, true, true);

        // Botão de calcular
        calcularBtn = new JButton("Calcular");
        calcularBtn.setBounds(340, 50, 100, 30);
        calcularBtn.addActionListener(this);
        calcularBtn.setFocusable(false);

        // Variáveis de saída
        tempoSubidaDescricao = "Tempo de subida (s): ";
        tempoSubidaOutput = criarTextField("", 30, 150, 410, 30, false, false);

        alturaMaxDescricao = "Altura máxima (m): ";
        alturaMaxOutput = criarTextField("", 30, 190, 410, 30, false, false);

        tempoTotalDescricao = "Tempo total de voo (s): ";
        tempoTotalOutput = criarTextField("", 30, 230, 410, 30, false, false);

        // Botão de resetar
        resetarBtn = new JButton("Resetar");
        resetarBtn.setBounds(200, 350, 100, 30);
        resetarBtn.addActionListener(this);
        resetarBtn.setFocusable(false);

        // Adiciona todos os elementos no frame
        frame.add(velocidadeLabel);
        frame.add(gravidadeLabel);
        frame.add(velocidadeInput);
        frame.add(gravidadeInput);
        frame.add(calcularBtn);
        frame.add(tempoSubidaOutput);
        frame.add(alturaMaxOutput);
        frame.add(tempoTotalOutput);
        frame.add(resetarBtn);

        frame.setVisible(true);
    }

    // Método main
    public static void main(String[] args) {
        new LancamentoVertical();
    }

    // Método que executa as ações dos botões
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == calcularBtn) {
            try {
                // Converte as entradas para double
                velocidadeInicial = Double.parseDouble(velocidadeInput.getText());
                gravidade = Double.parseDouble(gravidadeInput.getText());

                // Validações
                if (velocidadeInicial < 0) {
                    exibirErro("Velocidade inicial deve ser um valor positivo.");
                    return;
                }

                if (gravidade <= 0) {
                    exibirErro("A gravidade deve ser um valor positivo maior que zero.");
                    return;
                }

                if (gravidade > 50) {
                    exibirErro("Gravidade acima de 50 m/s² não é permitida neste simulador.");
                    return;
                }

                // Realiza os cálculos
                double tempoSubida = calcularTempoSubida(velocidadeInicial, gravidade);
                double alturaMaxima = calcularAlturaMaxima(velocidadeInicial, gravidade);
                double tempoTotal = calcularTempoTotal(tempoSubida);

                // Exibe resultados
                tempoSubidaOutput.setText(criarTextoSaida(tempoSubidaDescricao, tempoSubida));
                alturaMaxOutput.setText(criarTextoSaida(alturaMaxDescricao, alturaMaxima));
                tempoTotalOutput.setText(criarTextoSaida(tempoTotalDescricao, tempoTotal));

            } catch (NumberFormatException ex) {
                exibirErro("Por favor, insira valores numéricos válidos.");
            }
        }

        if (e.getSource() == resetarBtn) {
            limparCampos();
        }
    }

    // Cria TextFields
    private JTextField criarTextField(String texto, int x, int y, int w, int h, boolean editavel, boolean foco) {
        JTextField txt = new JTextField(texto);
        txt.setBounds(x, y, w, h);
        txt.setEditable(editavel);
        txt.setFocusable(foco);
        return txt;
    }

    // Cria Labels
    private JLabel criarLabel(String texto, int x, int y, int w, int h) {
        JLabel lbl = new JLabel(texto);
        lbl.setBounds(x, y, w, h);
        return lbl;
    }

    // Mostra mensagem de erro
    private void exibirErro(String msg) {
        JOptionPane.showMessageDialog(null, msg, "Erro", JOptionPane.ERROR_MESSAGE);
    }

    // Calcula tempo de subida
    private double calcularTempoSubida(double v0, double g) {
        return v0 / g;
    }

    // Calcula altura máxima
    private double calcularAlturaMaxima(double v0, double g) {
        return (v0 * v0) / (2 * g);
    }

    // Calcula tempo total de voo
    private double calcularTempoTotal(double tSubida) {
        return tSubida * 2;
    }

    // Cria string de resultado formatado
    private String criarTextoSaida(String descricao, double valor) {
        return descricao + String.format("%.2f", valor);
    }

    // Limpa todos os campos e variáveis
    private void limparCampos() {
        velocidadeInput.setText("");
        gravidadeInput.setText("9.8");
        tempoSubidaOutput.setText("");
        alturaMaxOutput.setText("");
        tempoTotalOutput.setText("");
        velocidadeInicial = null;
        gravidade = null;
    }
}
