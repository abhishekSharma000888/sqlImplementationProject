# sqlImplementationProject
A detailed project that implements SQL learnings and hands-on practice with relational tables.

-- Commands to Set Up the Database
-- 1. Clone the repository and navigate to the project folder
--    git clone <repo-url>
--    cd SDET_Testing_Platform

-- 2. Connect to MySQL and execute the SQL file
--    mysql -u <username> -p < database_name> < schema.sql

-- 3. Verify the tables
--    USE SDET_Testing_Platform;
--    SHOW TABLES;

-- 4. View Sample Data
--    SELECT * FROM Test_Projects;
--    SELECT * FROM Test_Cases;
--    SELECT * FROM Test_Executions;

-- SDET Testing Platform Database Schema

CREATE DATABASE SDET_Testing_Platform;
USE SDET_Testing_Platform;

-- Table to store information about test projects
CREATE TABLE Test_Projects (
    project_id INT AUTO_INCREMENT PRIMARY KEY,
    project_name VARCHAR(255) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Table to store test cases linked to test projects
CREATE TABLE Test_Cases (
    case_id INT AUTO_INCREMENT PRIMARY KEY,
    project_id INT,
    case_name VARCHAR(255) NOT NULL,
    description TEXT,
    test_type ENUM('Functional', 'Regression', 'Performance', 'Security') NOT NULL,
    priority ENUM('Low', 'Medium', 'High', 'Critical') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (project_id) REFERENCES Test_Projects(project_id) ON DELETE CASCADE
);

-- Table to store test executions with execution environment details
CREATE TABLE Test_Executions (
    execution_id INT AUTO_INCREMENT PRIMARY KEY,
    case_id INT,
    executed_by VARCHAR(255) NOT NULL,
    execution_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('Passed', 'Failed', 'Skipped', 'Blocked') NOT NULL,
    execution_env ENUM('Local', 'CI/CD', 'Staging', 'Production') NOT NULL,
    log TEXT,
    FOREIGN KEY (case_id) REFERENCES Test_Cases(case_id) ON DELETE CASCADE
);

-- Sample Data
INSERT INTO Test_Projects (project_name, description) VALUES
('E-commerce Platform', 'Testing the core features of an e-commerce platform'),
('Banking App', 'Automated testing for a banking application');

INSERT INTO Test_Cases (project_id, case_name, description, test_type, priority) VALUES
(1, 'User Login', 'Valid user login scenario', 'Functional', 'High'),
(1, 'Payment Processing', 'Ensure payments go through successfully', 'Regression', 'Critical'),
(2, 'Fund Transfer', 'Verify successful fund transfer', 'Functional', 'High'),
(2, 'Security Audit', 'Check for SQL injection vulnerabilities', 'Security', 'Critical');

INSERT INTO Test_Executions (case_id, executed_by, status, execution_env, log) VALUES
(1, 'AutomationBot', 'Passed', 'CI/CD', 'Login was successful'),
(2, 'AutomationBot', 'Failed', 'Staging', 'Payment gateway error'),
(3, 'AutomationBot', 'Passed', 'Production', 'Funds transferred successfully'),
(4, 'AutomationBot', 'Skipped', 'Local', 'Test skipped due to setup issue');
