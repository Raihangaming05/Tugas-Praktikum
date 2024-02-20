# Tugas-Praktikum
import java.util.Scanner;

public class Calculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Masukkan dua angka:");

        System.out.print("Angka pertama: ");
        double angka1 = scanner.nextDouble();

        System.out.print("Angka kedua: ");
        double angka2 = scanner.nextDouble();

        System.out.println("Pilih operasi (tambah/kurang/kali/bagi): ");
        String operasi = scanner.next();

        double hasil = hitung(angka1, angka2, operasi);

        System.out.println("Hasil: " + hasil);
    }

    private static double hitung(double angka1, double angka2, String operasi) {
        double hasil = 0.0;

        switch (operasi.toLowerCase()) {
            case "tambah":
                hasil = angka1 + angka2;
                break;
            case "kurang":
                hasil = angka1 - angka2;
                break;
            case "kali":
                hasil = angka1 * angka2;
                break;
            case "bagi":
                if (angka2 != 0) {
                    hasil = angka1 / angka2;
                } else {
                    System.out.println("Tidak bisa melakukan pembagian dengan nol.");
                }
                break;
            default:
                System.out.println("Operasi tidak valid.");
        }

        return hasil;
    }
}



import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CalculatorSwing {
    private JFrame frame;
    private JTextField textField;
    private double angka1, angka2, hasil;
    private String operasi;

    public static void main(String[] args) {
        EventQueue.invokeLater(() -> {
            try {
                CalculatorSwing window = new CalculatorSwing();
                window.frame.setVisible(true);
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
    }

    public CalculatorSwing() {
        frame = new JFrame();
        frame.setTitle("Kalkulator");
        frame.setBounds(100, 100, 300, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        frame.getContentPane().add(panel, BorderLayout.CENTER);
        panel.setLayout(new BorderLayout());

        textField = new JTextField();
        panel.add(textField, BorderLayout.NORTH);

        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(4, 4));

        String[] buttonLabels = {
                "7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", ".", "=", "+"
        };

        for (String label : buttonLabels) {
            JButton button = new JButton(label);
            button.addActionListener(new ButtonClickListener());
            buttonPanel.add(button);
        }

        panel.add(buttonPanel, BorderLayout.CENTER);
    }

    private class ButtonClickListener implements ActionListener {
        public void actionPerformed(ActionEvent event) {
            JButton source = (JButton) event.getSource();
            String buttonText = source.getText();

            if (buttonText.matches("[0-9]")) {
                textField.setText(textField.getText() + buttonText);
            } else if (buttonText.equals(".")) {
                if (!textField.getText().contains(".")) {
                    textField.setText(textField.getText() + buttonText);
                }
            } else if (buttonText.matches("[/*\\-+]")) {
                angka1 = Double.parseDouble(textField.getText());
                operasi = buttonText;
                textField.setText("");
            } else if (buttonText.equals("=")) {
                if (!textField.getText().isEmpty()) {
                    angka2 = Double.parseDouble(textField.getText());
                    hasil = hitung(angka1, angka2, operasi);
                    textField.setText(String.valueOf(hasil));
                }
            }
        }
    }

    private double hitung(double angka1, double angka2, String operasi) {
        switch (operasi) {
            case "+":
                return angka1 + angka2;
            case "-":
                return angka1 - angka2;
            case "*":
                return angka1 * angka2;
            case "/":
                if (angka2 != 0) {
                    return angka1 / angka2;
                } else {
                    JOptionPane.showMessageDialog(frame, "Tidak bisa melakukan pembagian dengan nol.", "Error", JOptionPane.ERROR_MESSAGE);
                    return 0;
                }
            default:
                return 0;
        }
    }
}
