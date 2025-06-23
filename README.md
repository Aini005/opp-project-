# opp-project-
Java GUI-Based Quiz Application – Project Report
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.List;
import java.io.*;

public class QuizApp extends JFrame {
    // Question model
    static class Question {
        String questionText;
        String[] options;
        int correctOptionIndex;

        Question(String q, String[] opts, int correct) {
            this.questionText = q;
            this.options = opts;
            this.correctOptionIndex = correct;
        }
    }

    // Quiz data
    private List<Question> questions = new ArrayList<>();
    private int currentQuestionIndex = 0;
    private int score = 0;
    private int timerSeconds = 15;
    private Timer countdownTimer;

    // GUI Components
    private JLabel questionLabel, timerLabel;
    private JRadioButton[] optionButtons = new JRadioButton[4];
    private ButtonGroup optionGroup = new ButtonGroup();
    private JButton nextButton;
    private JLabel highScoreLabel;

    // File for high score
    private final String HIGH_SCORE_FILE = "highscore.txt";

    public QuizApp() {
        loadQuestions();
        initGUI();
        showQuestion();
    }

    // Load sample questions
    private void loadQuestions() {
        questions.add(new Question("What is 2 + 2?", new String[]
        {"3", "4", "5", "6"}, 1));
        questions.add(new Question("Which planet is known as the Red Planet?", new String[]
        {"Earth", "Mars", "Jupiter", "Saturn"}, 1));
        questions.add(new Question("Who wrote 'Romeo and Juliet'?", new String[]
        {"Dickens", "Shakespeare", "Hemingway", "Twain"}, 1));
        questions.add(new Question("Capital of France?", new String[]
        {"Paris", "London", "Berlin", "Rome"}, 0));
    }

    // Initialize GUI
    private void initGUI() {
        setTitle("Java Quiz App");
        setSize(500, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Top Panel
        JPanel topPanel = new JPanel(new GridLayout(2, 1));
        timerLabel = new JLabel("Time Left: 15", JLabel.CENTER);
        timerLabel.setFont(new Font("Arial", Font.BOLD, 18));
        questionLabel = new JLabel("Question", JLabel.CENTER);
        questionLabel.setFont(new Font("Arial", Font.BOLD, 16));
        topPanel.add(timerLabel);
        topPanel.add(questionLabel);
        add(topPanel, BorderLayout.NORTH);

        // Center Options Panel
        JPanel optionsPanel = new JPanel(new GridLayout(4, 1));
        for (int i = 0; i < 4; i++) {
            optionButtons[i] = new JRadioButton();
            optionGroup.add(optionButtons[i]);
            optionsPanel.add(optionButtons[i]);
        }
        add(optionsPanel, BorderLayout.CENTER);

        // Bottom Panel
        JPanel bottomPanel = new JPanel(new BorderLayout());
        nextButton = new JButton("Next");
        highScoreLabel = new JLabel("High Score: " + getHighScore(), JLabel.CENTER);
        bottomPanel.add(highScoreLabel, BorderLayout.NORTH);
        bottomPanel.add(nextButton, BorderLayout.SOUTH);
        add(bottomPanel, BorderLayout.SOUTH);

        nextButton.addActionListener(e -> handleNext());

        setVisible(true);
    }

    // Show the current question
    private void showQuestion() {
        if (currentQuestionIndex >= questions.size()) {
            endQuiz();
            return;
        }

        Question q = questions.get(currentQuestionIndex);
        questionLabel.setText("Q" + (currentQuestionIndex + 1) + ": " + q.questionText);
        for (int i = 0; i < 4; i++) {
            optionButtons[i].setText(q.options[i]);
            optionButtons[i].setSelected(false);
        }

        // Reset and start timer
        if (countdownTimer != null) countdownTimer.stop();
        timerSeconds = 15;
        timerLabel.setText("Time Left: " + timerSeconds);
        countdownTimer = new Timer(1000, e -> {
            timerSeconds--;
            timerLabel.setText("Time Left: " + timerSeconds);
            if (timerSeconds == 0) {
                ((Timer) e.getSource()).stop();
                handleNext();  // Auto skip
            }
        });
        countdownTimer.start();
    }

    // Handle Next button or timeout
    private void handleNext() {
        countdownTimer.stop();
        Question q = questions.get(currentQuestionIndex);

        for (int i = 0; i < 4; i++) {
            if (optionButtons[i].isSelected()) {
                if (i == q.correctOptionIndex) {
                    score++;
                }
                break;
            }
        }

        currentQuestionIndex++;
        showQuestion();
    }

    // End the quiz
    private void endQuiz() {
        int highScore = getHighScore();
        if (score > highScore) {
            saveHighScore(score);
            highScoreLabel.setText("New High Score: " + score);
        } else {
            highScoreLabel.setText("High Score: " + highScore);
        }

        JOptionPane.showMessageDialog(this, "Quiz Over!\nYour Score: " 
        + score + "/" + questions.size());
        System.exit(0);
    }

    // Get high score from file
    private int getHighScore() {
        try (BufferedReader reader = new BufferedReader(new FileReader(HIGH_SCORE_FILE))) {
            return Integer.parseInt(reader.readLine());
        } catch (IOException | NumberFormatException e) {
            return 0;
        }
    }

    // Save new high score
    private void saveHighScore(int newScore) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(HIGH_SCORE_FILE))) {
            writer.write(String.valueOf(newScore));
        } catch (IOException e) {
            System.err.println("Failed to save high score.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new QuizApp());
    }
}
https://github.com/Aini005/opp-project-/commit/a137f3313d85ce18014d9d271fbc2b049c35037f#commitcomment-160640023
www.linkedin.com/in/aiñyē-khãn-b0901a28a
https://www.linkedin.com/posts/ai%C3%B1y%C4%93-kh%C3%A3n-b0901a28a_aini005-overview-activity-7342929774333313024-wrfh?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEYgOrkB6so03ngA91DveZ_GV1Jbs4R5148
