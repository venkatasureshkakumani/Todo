# Todo
Case Study to do application


-- Create database and user (run as root or a user with adequate privileges)
CREATE DATABASE IF NOT EXISTS todo_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create user (skip if you already have one)
CREATE USER IF NOT EXISTS 'todo'@'%' IDENTIFIED BY 'todopass';

-- Grant privileges
GRANT ALL PRIVILEGES ON todo_db.* TO 'todo'@'%';
FLUSH PRIVILEGES;

USE todo_db;

-- Tasks table (matches EF model TaskItem)
CREATE TABLE IF NOT EXISTS `Tasks` (
  `Id` INT NOT NULL AUTO_INCREMENT,
  `TaskName` VARCHAR(255) NOT NULL,
  `Description` TEXT NOT NULL,
  `StartDate` DATETIME(6) NOT NULL,
  `EndDate` DATETIME(6) NOT NULL,
  `Status` INT NOT NULL,   -- 0 = Pending, 1 = Completed
  `TotalEffort` INT NOT NULL,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `UX_Tasks_TaskName` (`TaskName`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Optional seed data
INSERT INTO `Tasks` (`TaskName`, `Description`, `StartDate`, `EndDate`, `Status`, `TotalEffort`) VALUES
('PrepareSlides', 'Create deck for demo',     UTC_TIMESTAMP() - INTERVAL 1 DAY, UTC_TIMESTAMP() + INTERVAL 2 DAY, 0, 8),
('FixBugs',       'Resolve P1 & P2 defects',  UTC_TIMESTAMP() - INTERVAL 2 DAY, UTC_TIMESTAMP() - INTERVAL 1 DAY, 1, 13),
('WriteTests',    'Increase coverage to 85%', UTC_TIMESTAMP(),                 UTC_TIMESTAMP() + INTERVAL 3 DAY, 0, 5)
ON DUPLICATE KEY UPDATE Description=VALUES(Description);
