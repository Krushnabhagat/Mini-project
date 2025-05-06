# Mini-project
applet code


    import java.awt.*;
    import java.awt.event.*;

    class LoginException extends Exception {
    public LoginException(String message) {
        super(message);
    }
    }

    public class LoginScreen extends Frame implements ActionListener {
    Label userLabel, passLabel, msgLabel;
    TextField userText, passText;
    Button loginButton, clearButton;
    int attempts = 3;

    public LoginScreen() {
        // Frame settings
        setTitle("Login Screen");
        setSize(300, 200);
        setLayout(new GridLayout(5, 2));
        setLocationRelativeTo(null);
        setVisible(true);

        // Create components
        userLabel = new Label("Username:");
        passLabel = new Label("Password:");
        msgLabel = new Label("");

        userText = new TextField(20);
        passText = new TextField(20);
        passText.setEchoChar('*');

        loginButton = new Button("Login");
        clearButton = new Button("Clear");

        // Add listeners
        loginButton.addActionListener(this);
        clearButton.addActionListener(this);

        // Add components to frame
        add(userLabel); add(userText);
        add(passLabel); add(passText);
        add(loginButton); add(clearButton);
        add(msgLabel);

        // Window close handler
        addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent we) {
                dispose();
            }
        });
    }

    public void actionPerformed(ActionEvent ae) {
        if (ae.getSource() == clearButton) {
            userText.setText("");
            passText.setText("");
            msgLabel.setText("");
        } else if (ae.getSource() == loginButton) {
            String username = userText.getText();
            String password = passText.getText();

            try {
                if (!username.equals(password)) {
                    attempts--;
                    throw new LoginException("Username and Password must be the same.");
                } else {
                    msgLabel.setText("Login Successful!");
                    loginButton.setEnabled(false);
                }
            } catch (LoginException le) {
                msgLabel.setText(le.getMessage() + " Attempts left: " + attempts);
                if (attempts == 0) {
                    msgLabel.setText("No attempts left. Login disabled.");
                    loginButton.setEnabled(false);
                }
            }
        }
    }

    public static void main(String[] args) {
        new LoginScreen();
    }
    }
