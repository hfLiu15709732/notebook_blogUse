```sql
CREATE SCHEMA `mobile_compare_app` ;




CREATE TABLE `mobile_compare_app`.`common_equipment` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `price` VARCHAR(255) NOT NULL,
  `cpu` VARCHAR(60) NOT NULL,
  `shortFunction` TEXT NOT NULL,
  `startTime` VARCHAR(60) NOT NULL,
  `displayTitle` VARCHAR(60) NOT NULL,
  `displayFunction` VARCHAR(255) NOT NULL,
  `sizeWeight` VARCHAR(255) NOT NULL,
  `displayChangeTo` VARCHAR(255) NULL,
  `outfit` VARCHAR(60) NULL,
  `cameraFront` VARCHAR(255) NULL,
  `cameraBack` TEXT NULL,
  `cameraFunction` VARCHAR(255) NULL,
  `bateryAll` VARCHAR(45) NOT NULL,
  `bateryCharge` VARCHAR(80) NOT NULL,
  `storageLevel` VARCHAR(45) NOT NULL,
  `systemOS` VARCHAR(45) NOT NULL,
  `brandAll` VARCHAR(45) NOT NULL,
  `motorMobile` VARCHAR(45) NOT NULL,
  `otherSensors` VARCHAR(255) NOT NULL,
  `cpuDashBoard` TEXT NOT NULL,
  `GpuDashBoard` TEXT NOT NULL,
  `dashBoardContent` VARCHAR(80) NOT NULL,
  `DxoMark` VARCHAR(45) NOT NULL,
  `dataConnect` VARCHAR(45) NULL,
  `musicConnect` VARCHAR(45) NULL,
  `websiteConnect` VARCHAR(45) NULL,
  `blueTooth` VARCHAR(45) NULL,
  `isNFC` INT NOT NULL,
  `isIRDA` INT NOT NULL,
  `navigatorSystem` VARCHAR(255) NOT NULL,
  `simCardType` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE);
  
  
  
  
  
  CREATE TABLE `mobile_compare_app`.`mobilecpuinfo` (
  `mID` INT NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(45) NOT NULL DEFAULT '名称',
  `brand` VARCHAR(45) NOT NULL DEFAULT '所属品牌',
  `cpuDetail` TEXT NOT NULL DEFAULT 'CPU规格',
  `gpuDetail` TEXT NOT NULL DEFAULT 'GPU规格',
  `startTime` VARCHAR(45) NOT NULL DEFAULT '发布时间',
  `cpuDashBoard` TEXT NOT NULL DEFAULT 'CPU量化性能',
  `gpuDashBoard` TEXT NOT NULL DEFAULT 'GPU量化性能',
  `cpuContent` TEXT NOT NULL DEFAULT '处理器整体性能',
  `highLoading` TEXT NOT NULL DEFAULT '高负载功耗',
  PRIMARY KEY (`mID`),
  UNIQUE INDEX `id_UNIQUE` (`mID` ASC) VISIBLE);






CREATE TABLE `mobile_compare_app`.`mangertab` (
  `userID` INT NOT NULL AUTO_INCREMENT,
  `userName` VARCHAR(45) NOT NULL,
  `password` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`userID`),
  UNIQUE INDEX `userID_UNIQUE` (`userID` ASC) VISIBLE);
```









