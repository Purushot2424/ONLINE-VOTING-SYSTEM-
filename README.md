# ONLINE-VOTING-SYSTEM-
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.HashSet;

public class OnlineVotingSystem {
    // Candidate names
    private static final String[] candidates = {"Candidate A", "Candidate B", "Candidate C"};
    private static final HashMap<String, Integer> votes = new HashMap<>(); // Stores votes for candidates
    private static final HashSet<String> voters = new HashSet<>(); // Keeps track of voters to ensure unique voting

    public static void main(String[] args) {
        // Initialize vote counts for each candidate
        for (String candidate : candidates) {
            votes.put(candidate, 0);
        }

        // Create the main frame
        JFrame frame = new JFrame("Online Voting System");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500, 400);
        frame.setLayout(new BorderLayout());

        // Title Label
        JLabel title = new JLabel("Vote for Your Candidate", JLabel.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 18));
        frame.add(title, BorderLayout.NORTH);

        // Panel for user input
        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridLayout(3, 2, 10, 10));

        // Voter name input
        JLabel voterLabel = new JLabel("Enter Your Name:");
        JTextField voterNameField = new JTextField();
        inputPanel.add(voterLabel);
        inputPanel.add(voterNameField);

        // Candidate selection
        JLabel candidateLabel = new JLabel("Choose Your Candidate:");
        JComboBox<String> candidateComboBox = new JComboBox<>(candidates);
        inputPanel.add(candidateLabel);
        inputPanel.add(candidateComboBox);

        // Vote button
        JButton voteButton = new JButton("Vote");
        inputPanel.add(new JLabel()); // Empty cell for spacing
        inputPanel.add(voteButton);
        frame.add(inputPanel, BorderLayout.CENTER);

        // Results Button
        JButton resultsButton = new JButton("Show Results");
        resultsButton.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(resultsButton, BorderLayout.SOUTH);

        // ActionListener for the vote button
        voteButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String voterName = voterNameField.getText().trim();
                String chosenCandidate = (String) candidateComboBox.getSelectedItem();

                if (voterName.isEmpty()) {
                    JOptionPane.showMessageDialog(frame, "Please enter your name!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }

                if (voters.contains(voterName)) {
                    JOptionPane.showMessageDialog(frame, "You have already voted!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }

                // Record the vote
                voters.add(voterName);
                votes.put(chosenCandidate, votes.get(chosenCandidate) + 1);

                JOptionPane.showMessageDialog(frame, "Thank you for voting!", "Success", JOptionPane.INFORMATION_MESSAGE);
                voterNameField.setText(""); // Clear the voter name field
            }
        });

        // ActionListener for the results button
        resultsButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // Display voting results
                StringBuilder resultMessage = new StringBuilder("Voting Results:\n");
                for (String candidate : candidates) {
                    resultMessage.append(candidate).append(": ").append(votes.get(candidate)).append(" votes\n");
                }

                JOptionPane.showMessageDialog(frame, resultMessage.toString(), "Results", JOptionPane.INFORMATION_MESSAGE);
            }
        });

        // Show the frame
        frame.setVisible(true);
    }
}
