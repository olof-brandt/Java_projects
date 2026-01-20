

//This Java program connects to a MySQL database, retrieves measurement data within a specific date range, optionally displays the data in the console, and visualizes it using a simple scatter or line plot using Java Swing. The code demonstrates JDBC database connection, data processing, and custom plotting within a Swing GUI.


Copy
package com.example.MysqlTestJava07;

import java.sql.*;
import java.util.*;
import javax.swing.*;
import java.awt.*;
import java.awt.geom.*; // For drawing shapes like Ellipse2D

/**
 * This program connects to a MySQL database, retrieves measurement data within a specified date range,
 * optionally prints the data, and visualizes it using a simple plot in a Swing window.
 */
public class MysqlTestJava07 {

    public static void main(String[] args) {
        // Configuration flags
        boolean printData = true;
        boolean toArray = true;
        boolean plotData = true;

        // List to hold the retrieved data
        List<Double> dataPoints = new ArrayList<>();

        // Database connection parameters
        String url = "jdbc:mysql://mysql683.loopia.se/sololof_se_db_1";
        String user = "github@s266820";
        String password = "gatsby1925";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("Database is connected!");

            String query = "SELECT timeDB, ApparentPower FROM Measurements "
                         + "WHERE timeDB > '2020-11-05' AND timeDB < '2020-11-06'";

            try (PreparedStatement stmt = conn.prepareStatement(query);
                 ResultSet rs = stmt.executeQuery()) {

                if (toArray) {
                    while (rs.next()) {
                        // Assuming ApparentPower is a numeric value
                        double power = rs.getDouble("ApparentPower");
                        dataPoints.add(power);
                    }
                }

                if (printData) {
                    for (Double val : dataPoints) {
                        System.out.println(val);
                    }
                }
            }
        } catch (Exception e) {
            System.out.println("Error connecting to the database: " + e);
            return; // Exit if database connection fails
        }

        // Plot the data if enabled
        if (plotData) {
            SwingUtilities.invokeLater(() -> {
                JFrame frame = new JFrame("Data Plot");
                frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
                PlotPanel plotPanel = new PlotPanel(dataPoints);
                frame.add(plotPanel);
                frame.setSize(800, 600);
                frame.setLocation(200, 200);
                frame.setVisible(true);
            });
        }
    }

    /**
     * Custom JPanel for plotting data points either as scatter or line plot.
     */
    public static class PlotPanel extends JPanel {
        private final List<Double> data;
        private final boolean scatter = false; // Set to true for scatter plot

        // Constructor
        public PlotPanel(List<Double> data) {
            this.data = data;
        }

        @Override
        protected void paintComponent(Graphics g) {
            super.paintComponent(g);
            Graphics2D g2 = (Graphics2D) g;

            // Enable anti-aliasing for smoother graphics
            g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);

            int width = getWidth();
            int height = getHeight();
            int margin = 60;

            // Draw axes
            g2.draw(new Line2D.Double(margin, margin, margin, height - margin));
            g2.draw(new Line2D.Double(margin, height - margin, width - margin, height - margin));

            if (data.isEmpty()) {
                return; // Nothing to plot
            }

            // Calculate scaling factors
            double xInterval = (double) (width - 2 * margin) / (data.size() - 1);
            double maxVal = getMax();
            double scaleY = (double) (height - 2 * margin) / maxVal;

            g2.setPaint(Color.RED);

            if (scatter) {
                // Plot scatter points
                for (int i = 0; i < data.size(); i++) {
                    double x = margin + i * xInterval;
                    double y = height - margin - data.get(i) * scaleY;
                    g2.fill(new Ellipse2D.Double(x - 2, y - 2, 4, 4));
                }
            } else {
                // Plot line graph
                int xPrev = margin;
                int yPrev = height - margin - (int) (data.get(0) * scaleY);
                for (int i = 1; i < data.size(); i++) {
                    int x = (int) (margin + i * xInterval);
                    int y = height - margin - (int) (data.get(i) * scaleY);
                    g2.drawLine(xPrev, yPrev, x, y);
                    xPrev = x;
                    yPrev = y;
                }
            }
        }

        /**
         * Find maximum value in data to scale the plot.
         */
         //
        private double getMax() {
            return Collections.max(data);
        }
    }
}