# RobloxiaPlus-
It is kind of like roblox
import java.awt.*;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;

public class RobloxiaPlus extends JFrame {

    enum Screen {
        HOME,
        EDITOR
    }

    enum ObjectType {
        TREE, BUILDING, LAKE, BRIDGE
    }

    static class WorldObject {
        int x, y;
        ObjectType type;

        WorldObject(int x, int y, ObjectType type) {
            this.x = x;
            this.y = y;
            this.type = type;
        }
    }

    ArrayList<ArrayList<WorldObject>> worlds = new ArrayList<>();
    ArrayList<WorldObject> currentWorld = new ArrayList<>();

    Screen screen = Screen.HOME;
    ObjectType selected = ObjectType.TREE;

    public RobloxiaPlus() {

        setTitle("Robloxia Plus");
        setSize(1400, 900);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        GamePanel panel = new GamePanel();
        add(panel);

        setVisible(true);
    }

    class GamePanel extends JPanel {

        Rectangle createButton =
                new Rectangle(620, 300, 160, 160);

        Rectangle treeButton =
                new Rectangle(20, 70, 120, 40);

        Rectangle buildingButton =
                new Rectangle(20, 120, 120, 40);

        Rectangle lakeButton =
                new Rectangle(20, 170, 120, 40);

        Rectangle bridgeButton =
                new Rectangle(20, 220, 120, 40);

        Rectangle submitButton =
                new Rectangle(1150, 760, 180, 50);

        public GamePanel() {

            setBackground(Color.BLACK);

            addMouseListener(new MouseAdapter() {

                @Override
                public void mousePressed(MouseEvent e) {

                    int x = e.getX();
                    int y = e.getY();

                    if (screen == Screen.HOME) {

                        if (createButton.contains(x, y)) {
                            currentWorld.clear();
                            screen = Screen.EDITOR;
                        }
                    } else {

                        if (treeButton.contains(x, y))
                            selected = ObjectType.TREE;

                        else if (buildingButton.contains(x, y))
                            selected = ObjectType.BUILDING;

                        else if (lakeButton.contains(x, y))
                            selected = ObjectType.LAKE;

                        else if (bridgeButton.contains(x, y))
                            selected = ObjectType.BRIDGE;

                        else if (submitButton.contains(x, y)) {

                            worlds.add(
                                    new ArrayList<>(currentWorld));

                            screen = Screen.HOME;
                        }

                        else if (x > 180 && x < 1100 &&
                                 y > 50 && y < 700) {

                            currentWorld.add(
                                    new WorldObject(
                                            x,
                                            y,
                                            selected));
                        }
                    }

                    repaint();
                }
            });
        }

        @Override
        protected void paintComponent(Graphics g) {

            super.paintComponent(g);

            Graphics2D g2 =
                    (Graphics2D) g;

            g2.setRenderingHint(
                    RenderingHints.KEY_ANTIALIASING,
                    RenderingHints.VALUE_ANTIALIAS_ON);

            if (screen == Screen.HOME)
                drawHome(g2);
            else
                drawEditor(g2);
        }

        private void drawHome(Graphics2D g) {

            g.setColor(Color.WHITE);
            g.setFont(
                    new Font(
                            "Arial",
                            Font.BOLD,
                            36));

            g.drawString(
                    "Robloxia Plus",
                    20,
                    50);

            // Profile Card

            g.setColor(
                    new Color(30, 30, 30));

            g.fillRoundRect(
                    1080,
                    120,
                    260,
                    320,
                    30,
                    30);

            g.setColor(Color.CYAN);

            g.fillOval(
                    1160,
                    150,
                    100,
                    100);

            g.setColor(Color.WHITE);

            g.drawString(
                    "Player",
                    1140,
                    300);

            g.setFont(
                    new Font(
                            "Arial",
                            Font.PLAIN,
                            20));

            g.drawString(
                    "Joined: 2026",
                    1125,
                    340);

            // Main Plus Button

            g.setColor(
                    new Color(0, 140, 255));

            g.fillOval(
                    createButton.x,
                    createButton.y,
                    createButton.width,
                    createButton.height);

            g.setColor(Color.WHITE);

            g.setFont(
                    new Font(
                            "Arial",
                            Font.BOLD,
                            90));

            g.drawString(
                    "+",
                    675,
                    410);

            // Worlds

            int cols = 4;

            for (int i = 0; i < worlds.size(); i++) {

                int x =
                        40 +
                        (i % cols) * 250;

                int y =
                        560 +
                        (i / cols) * 120;

                g.setColor(
                        new Color(60, 60, 60));

                g.fillRoundRect(
                        x,
                        y,
                        200,
                        100,
                        20,
                        20);

                g.setColor(Color.WHITE);

                g.drawString(
                        "World " + (i + 1),
                        x + 40,
                        y + 55);
            }
        }

        private void drawEditor(Graphics2D g) {

            g.setColor(Color.WHITE);

            g.setFont(
                    new Font(
                            "Arial",
                            Font.BOLD,
                            30));

            g.drawString(
                    "World Builder",
                    20,
                    40);

            drawButton(
                    g,
                    treeButton,
                    "Tree");

            drawButton(
                    g,
                    buildingButton,
                    "Building");

            drawButton(
                    g,
                    lakeButton,
                    "Lake");

            drawButton(
                    g,
                    bridgeButton,
                    "Bridge");

            g.setColor(
                    new Color(25,25,25));

            g.fillRect(
                    180,
                    50,
                    1000,
                    650);

            for (WorldObject obj :
                    currentWorld) {

                switch (obj.type) {

                    case TREE:
                        g.setColor(
                                Color.GREEN);
                        g.fillOval(
                                obj.x,
                                obj.y,
                                25,
                                25);
                        break;

                    case BUILDING:
                        g.setColor(
                                Color.YELLOW);
                        g.fillRect(
                                obj.x,
                                obj.y,
                                30,
                                30);
                        break;

                    case LAKE:
                        g.setColor(
                                Color.BLUE);
                        g.fillOval(
                                obj.x,
                                obj.y,
                                40,
                                35);
                        break;

                    case BRIDGE:
                        g.setColor(
                                new Color(
                                        120,
                                        80,
                                        30));
                        g.fillRect(
                                obj.x,
                                obj.y,
                                45,
                                15);
                        break;
                }
            }

            g.setColor(
                    new Color(0,200,80));

            g.fillRoundRect(
                    submitButton.x,
                    submitButton.y,
                    submitButton.width,
                    submitButton.height,
                    20,
                    20);

            g.setColor(Color.WHITE);

            g.drawString(
                    "Submit",
                    1195,
                    795);
        }

        private void drawButton(
                Graphics2D g,
                Rectangle r,
                String text) {

            g.setColor(
                    new Color(50,50,50));

            g.fillRoundRect(
                    r.x,
                    r.y,
                    r.width,
                    r.height,
                    20,
                    20);

            g.setColor(Color.WHITE);

            g.drawString(
                    text,
                    r.x + 20,
                    r.y + 28);
        }
    }

    public static void main(String[] args) {

        SwingUtilities.invokeLater(
                RobloxiaPlus::new);
    }
}

