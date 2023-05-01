# JAVA-GUI
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class GuessingGame extends JFrame implements ActionListener {

    private static final int MAX_GUESSES = 7;
    private int numberToGuess;
    private int remainingGuesses;

    private JTextField guessField;
    private JLabel resultLabel;
    private JButton guessButton;

    public GuessingGame() {
        // set up the frame
        setTitle("Guessing Game");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setResizable(false);
        setSize(300, 150);
        setLocationRelativeTo(null);

        // set up the components
        JLabel titleLabel = new JLabel("Guess a number between 1 and 100:");
        guessField = new JTextField(10);
        guessField.addActionListener(this);
        resultLabel = new JLabel("");
        guessButton = new JButton("Guess");
        guessButton.addActionListener(this);

        // add components to the frame
        JPanel mainPanel = new JPanel(new GridLayout(3, 1));
        mainPanel.add(titleLabel);
        mainPanel.add(guessField);
        mainPanel.add(resultLabel);
        getContentPane().add(mainPanel, BorderLayout.CENTER);
        getContentPane().add(guessButton, BorderLayout.SOUTH);

        // start a new game
        newGame();
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        int guess = 0;
        try {
            guess = Integer.parseInt(guessField.getText());
        } catch (NumberFormatException ex) {
            resultLabel.setText("Invalid input. Please enter a number.");
            guessField.setText("");
            guessField.requestFocus();
            return;
        }

        if (guess < 1 || guess > 100) {
            resultLabel.setText("Number must be between 1 and 100.");
            guessField.setText("");
            guessField.requestFocus();
            return;
        }

        if (guess == numberToGuess) {
            resultLabel.setText("Congratulations, you guessed the number!");
            guessField.setEditable(false);
            guessButton.setEnabled(false);
        } else {
            remainingGuesses--;
            if (remainingGuesses == 0) {
                resultLabel.setText("Sorry, you have run out of guesses. The number was " + numberToGuess + ".");
                guessField.setEditable(false);
                guessButton.setEnabled(false);
            } else {
                String hint = guess < numberToGuess ? "higher" : "lower";
                resultLabel.setText("Incorrect. The number is " + hint + ". You have " + remainingGuesses + " guesses left.");
                guessField.setText("");
                guessField.requestFocus();
            }
        }
    }

    private void newGame() {
        numberToGuess = (int)(Math.random() * 100) + 1;
        remainingGuesses = MAX_GUESSES;
        guessField.setEditable(true);
        guessButton.setEnabled(true);
        guessField.setText("");
        guessField.requestFocus();
        resultLabel.setText("");
    }

    public static void main(String[] args) {
        GuessingGame game = new GuessingGame();
        game.setVisible(true);
    }
}
