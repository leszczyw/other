# random

# Convert to csv from Excel file in Python

	import xlrd
	import csv
	import unicodecsv
	
	wb = xlrd.open_workbook('input_file.xlsx')
	sh = wb.sheet_by_index(0)
	
	fh = open('output_file.csv',"wb")
	csv_out = unicodecsv.writer(fh, encoding='utf-8-sig', lineterminator='\n', quotechar = '"', quoting=csv.QUOTE_NONNUMERIC)
	
	for row_number in range (sh.nrows):
	    csv_out.writerow(sh.row_values(row_number))
	
	fh.close()

text = open("output_file", "r", encoding='utf8')
text = ''.join([i for i in text]).replace(".0", "")
text = ''.join([i for i in text]).replace("ï»¿", "")
x = open("output_file","w", encoding='utf8')
x.writelines(text)
x.close()

# Change column type in PostgreSQL

ALTER TABLE table_name ALTER COLUMN column_name SET DATA TYPE timestamp USING column_name::timestamp without time zone

# Connector between MSSQL and PostgreSQL database

import pymssql, psycopg2, sys

class DatabaseRequest:

    def __init__(self):
        self.conn1 = pymssql.connect(host='server_name', port='1434', database='database_name', user='user_name', password='password')
        self.conn2 = psycopg2.connect(host='server_name', port='5432', database='database_name', user='user_name', password='password')
        
        self.cur1 = self.conn1.cursor()
        print("MSSQL DB connected")

    def request_proc(self):
        self.cur1.execute("SELECT * FROM table_name_from_mssql")
        rows = self.cur1.fetchall()
        print(rows)
        print("Data from MSSQL downloaded") 

        self.cur2 = self.conn2.cursor()
        print("PGSQL DB connected")
        
        for row in rows:
            self.cur2.execute('''
			INSERT INTO table_name (project_id, project_name, proj_group_id) 
			VALUES (%s, %s, %s)''', row)
        print('Data inserted')

        self.conn1.commit()
        self.conn2.commit()
        
    def close_cur(self):
        if self.cur1:
            self.cur1.close()
        elif self.cur2:
            self.cur2.close()

    def close_conn(self):
        if self.conn1:
            self.conn1.close()
        elif self.conn2:
            self.conn2.close()

if __name__ == '__main__':
    try:
        a = DatabaseRequest()
        a.request_proc()
    except (Exception, psycopg2.Error) as error:
        print(f"Error desc: {error}")
    finally:
        a.close_cur()
        a.close_conn()
        
# Connector between IBM Informix and PostgreSQL database

import sys
import logging
from pprint import pprint
import os
import jaydebeapi
import psycopg2

# CONFIG FOR INFORMIX
dsn_database = "database_name"
dsn_hostname = "host_name"
dsn_port = "port"
dsn_protocol = "TCPIP"
dsn_uid = "user_name"
dsn_pwd = "user_password"
dsn_drv_name = "com.informix.jdbc.IfxDriver"
dsn_drv_file = "./ifxjdbc.jar"
dsn_url = "jdbc:informix-sqli://host_name:port/database_name:INFORMIXSERVER=ids_name;CLIENT_LOCALE=pl_pl.port;NEWCODESET=mazovia,mazovia,port"

# CONFIG FOR POSTGRESQL
pg_conn = psycopg2.connect(database='database_name', user='user_name', password='password', host='host_name', port='5432')
pg_cur = pg_conn.cursor()

...

if __name__ == '__main__':
    main()

# NIFI: simple load csv to PostgreSQL

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<template encoding-version="1.2">
    <description></description>
    <groupId>edd5ca27-016d-1000-f910-b874992b16db</groupId>
    <name>csv_to_postgres</name>
    <snippet>
        <connections>
            <id>33ee143a-a788-35e9-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <backPressureDataSizeThreshold>1 GB</backPressureDataSizeThreshold>
            <backPressureObjectThreshold>10000</backPressureObjectThreshold>
            <destination>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>609c6845-640a-3edc-0000-000000000000</id>
                <type>PROCESSOR</type>
            </destination>
            <flowFileExpiration>0 sec</flowFileExpiration>
            <labelIndex>1</labelIndex>
            <loadBalanceCompression>DO_NOT_COMPRESS</loadBalanceCompression>
            <loadBalancePartitionAttribute></loadBalancePartitionAttribute>
            <loadBalanceStatus>LOAD_BALANCE_NOT_CONFIGURED</loadBalanceStatus>
            <loadBalanceStrategy>DO_NOT_LOAD_BALANCE</loadBalanceStrategy>
            <name></name>
            <selectedRelationships>success</selectedRelationships>
            <source>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>3460a102-c24d-3e37-0000-000000000000</id>
                <type>PROCESSOR</type>
            </source>
            <zIndex>0</zIndex>
        </connections>
        <connections>
            <id>4ce1cfbe-40b9-3a0c-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <backPressureDataSizeThreshold>1 GB</backPressureDataSizeThreshold>
            <backPressureObjectThreshold>10000</backPressureObjectThreshold>
            <destination>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>a3f416b4-ae35-3d0f-0000-000000000000</id>
                <type>PROCESSOR</type>
            </destination>
            <flowFileExpiration>0 sec</flowFileExpiration>
            <labelIndex>1</labelIndex>
            <loadBalanceCompression>DO_NOT_COMPRESS</loadBalanceCompression>
            <loadBalancePartitionAttribute></loadBalancePartitionAttribute>
            <loadBalanceStatus>LOAD_BALANCE_NOT_CONFIGURED</loadBalanceStatus>
            <loadBalanceStrategy>DO_NOT_LOAD_BALANCE</loadBalanceStrategy>
            <name></name>
            <selectedRelationships>sql</selectedRelationships>
            <source>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>b9b34187-d5c4-3730-0000-000000000000</id>
                <type>PROCESSOR</type>
            </source>
            <zIndex>0</zIndex>
        </connections>
        <connections>
            <id>7c55f686-f922-32d5-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <backPressureDataSizeThreshold>1 GB</backPressureDataSizeThreshold>
            <backPressureObjectThreshold>10000</backPressureObjectThreshold>
            <destination>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>b9b34187-d5c4-3730-0000-000000000000</id>
                <type>PROCESSOR</type>
            </destination>
            <flowFileExpiration>0 sec</flowFileExpiration>
            <labelIndex>1</labelIndex>
            <loadBalanceCompression>DO_NOT_COMPRESS</loadBalanceCompression>
            <loadBalancePartitionAttribute></loadBalancePartitionAttribute>
            <loadBalanceStatus>LOAD_BALANCE_NOT_CONFIGURED</loadBalanceStatus>
            <loadBalanceStrategy>DO_NOT_LOAD_BALANCE</loadBalanceStrategy>
            <name></name>
            <selectedRelationships>success</selectedRelationships>
            <source>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>500e2a2e-6b1f-3aa0-0000-000000000000</id>
                <type>PROCESSOR</type>
            </source>
            <zIndex>0</zIndex>
        </connections>
        <connections>
            <id>afae88cf-c775-3583-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <backPressureDataSizeThreshold>1 GB</backPressureDataSizeThreshold>
            <backPressureObjectThreshold>10000</backPressureObjectThreshold>
            <destination>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>500e2a2e-6b1f-3aa0-0000-000000000000</id>
                <type>PROCESSOR</type>
            </destination>
            <flowFileExpiration>0 sec</flowFileExpiration>
            <labelIndex>1</labelIndex>
            <loadBalanceCompression>DO_NOT_COMPRESS</loadBalanceCompression>
            <loadBalancePartitionAttribute></loadBalancePartitionAttribute>
            <loadBalanceStatus>LOAD_BALANCE_NOT_CONFIGURED</loadBalanceStatus>
            <loadBalanceStrategy>DO_NOT_LOAD_BALANCE</loadBalanceStrategy>
            <name></name>
            <selectedRelationships>success</selectedRelationships>
            <source>
                <groupId>78ed13ea-8e77-355e-0000-000000000000</groupId>
                <id>609c6845-640a-3edc-0000-000000000000</id>
                <type>PROCESSOR</type>
            </source>
            <zIndex>0</zIndex>
        </connections>
        <controllerServices>
            <id>d20851ae-43ce-37d5-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <bundle>
                <artifact>nifi-dbcp-service-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <comments></comments>
            <descriptors>
                <entry>
                    <key>Database Connection URL</key>
                    <value>
                        <name>Database Connection URL</name>
                    </value>
                </entry>
                <entry>
                    <key>Database Driver Class Name</key>
                    <value>
                        <name>Database Driver Class Name</name>
                    </value>
                </entry>
                <entry>
                    <key>database-driver-locations</key>
                    <value>
                        <name>database-driver-locations</name>
                    </value>
                </entry>
                <entry>
                    <key>kerberos-credentials-service</key>
                    <value>
                        <identifiesControllerService>org.apache.nifi.kerberos.KerberosCredentialsService</identifiesControllerService>
                        <name>kerberos-credentials-service</name>
                    </value>
                </entry>
                <entry>
                    <key>Database User</key>
                    <value>
                        <name>Database User</name>
                    </value>
                </entry>
                <entry>
                    <key>Password</key>
                    <value>
                        <name>Password</name>
                    </value>
                </entry>
                <entry>
                    <key>Max Wait Time</key>
                    <value>
                        <name>Max Wait Time</name>
                    </value>
                </entry>
                <entry>
                    <key>Max Total Connections</key>
                    <value>
                        <name>Max Total Connections</name>
                    </value>
                </entry>
                <entry>
                    <key>Validation-query</key>
                    <value>
                        <name>Validation-query</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-min-idle-conns</key>
                    <value>
                        <name>dbcp-min-idle-conns</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-max-idle-conns</key>
                    <value>
                        <name>dbcp-max-idle-conns</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-max-conn-lifetime</key>
                    <value>
                        <name>dbcp-max-conn-lifetime</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-time-between-eviction-runs</key>
                    <value>
                        <name>dbcp-time-between-eviction-runs</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-min-evictable-idle-time</key>
                    <value>
                        <name>dbcp-min-evictable-idle-time</name>
                    </value>
                </entry>
                <entry>
                    <key>dbcp-soft-min-evictable-idle-time</key>
                    <value>
                        <name>dbcp-soft-min-evictable-idle-time</name>
                    </value>
                </entry>
            </descriptors>
            <name>PGSQLConnectionPool</name>
            <persistsState>false</persistsState>
            <properties>
                <entry>
                    <key>Database Connection URL</key>
                    <value>jdbc:postgresql://localhost:5432/postgres</value>
                </entry>
                <entry>
                    <key>Database Driver Class Name</key>
                    <value>org.postgresql.Driver</value>
                </entry>
                <entry>
                    <key>database-driver-locations</key>
                    <value>file:///C:/Program Files/PostgreSQL/11/share/java/postgresql-42.2.6.jar</value>
                </entry>
                <entry>
                    <key>kerberos-credentials-service</key>
                </entry>
                <entry>
                    <key>Database User</key>
                    <value>postgres</value>
                </entry>
                <entry>
                    <key>Password</key>
                </entry>
                <entry>
                    <key>Max Wait Time</key>
                    <value>500 millis</value>
                </entry>
                <entry>
                    <key>Max Total Connections</key>
                    <value>8</value>
                </entry>
                <entry>
                    <key>Validation-query</key>
                </entry>
                <entry>
                    <key>dbcp-min-idle-conns</key>
                    <value>0</value>
                </entry>
                <entry>
                    <key>dbcp-max-idle-conns</key>
                    <value>8</value>
                </entry>
                <entry>
                    <key>dbcp-max-conn-lifetime</key>
                    <value>-1</value>
                </entry>
                <entry>
                    <key>dbcp-time-between-eviction-runs</key>
                    <value>-1</value>
                </entry>
                <entry>
                    <key>dbcp-min-evictable-idle-time</key>
                    <value>30 mins</value>
                </entry>
                <entry>
                    <key>dbcp-soft-min-evictable-idle-time</key>
                    <value>-1</value>
                </entry>
            </properties>
            <state>ENABLED</state>
            <type>org.apache.nifi.dbcp.DBCPConnectionPool</type>
        </controllerServices>
        <processors>
            <id>3460a102-c24d-3e37-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <position>
                <x>4.639170891637946</x>
                <y>0.13751692525983117</y>
            </position>
            <bundle>
                <artifact>nifi-standard-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <config>
                <bulletinLevel>WARN</bulletinLevel>
                <comments></comments>
                <concurrentlySchedulableTaskCount>1</concurrentlySchedulableTaskCount>
                <descriptors>
                    <entry>
                        <key>Input Directory</key>
                        <value>
                            <name>Input Directory</name>
                        </value>
                    </entry>
                    <entry>
                        <key>File Filter</key>
                        <value>
                            <name>File Filter</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Path Filter</key>
                        <value>
                            <name>Path Filter</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Batch Size</key>
                        <value>
                            <name>Batch Size</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Keep Source File</key>
                        <value>
                            <name>Keep Source File</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Recurse Subdirectories</key>
                        <value>
                            <name>Recurse Subdirectories</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Polling Interval</key>
                        <value>
                            <name>Polling Interval</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Ignore Hidden Files</key>
                        <value>
                            <name>Ignore Hidden Files</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Minimum File Age</key>
                        <value>
                            <name>Minimum File Age</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Maximum File Age</key>
                        <value>
                            <name>Maximum File Age</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Minimum File Size</key>
                        <value>
                            <name>Minimum File Size</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Maximum File Size</key>
                        <value>
                            <name>Maximum File Size</name>
                        </value>
                    </entry>
                </descriptors>
                <executionNode>ALL</executionNode>
                <lossTolerant>false</lossTolerant>
                <penaltyDuration>30 sec</penaltyDuration>
                <properties>
                    <entry>
                        <key>Input Directory</key>
                        <value>C:\Users\wojciechle\Downloads\dane</value>
                    </entry>
                    <entry>
                        <key>File Filter</key>
                        <value>[^\.].*\.csv</value>
                    </entry>
                    <entry>
                        <key>Path Filter</key>
                    </entry>
                    <entry>
                        <key>Batch Size</key>
                        <value>10</value>
                    </entry>
                    <entry>
                        <key>Keep Source File</key>
                        <value>true</value>
                    </entry>
                    <entry>
                        <key>Recurse Subdirectories</key>
                        <value>true</value>
                    </entry>
                    <entry>
                        <key>Polling Interval</key>
                        <value>0 sec</value>
                    </entry>
                    <entry>
                        <key>Ignore Hidden Files</key>
                        <value>true</value>
                    </entry>
                    <entry>
                        <key>Minimum File Age</key>
                        <value>0 sec</value>
                    </entry>
                    <entry>
                        <key>Maximum File Age</key>
                    </entry>
                    <entry>
                        <key>Minimum File Size</key>
                        <value>0 B</value>
                    </entry>
                    <entry>
                        <key>Maximum File Size</key>
                    </entry>
                </properties>
                <runDurationMillis>0</runDurationMillis>
                <schedulingPeriod>0 sec</schedulingPeriod>
                <schedulingStrategy>TIMER_DRIVEN</schedulingStrategy>
                <yieldDuration>1 sec</yieldDuration>
            </config>
            <executionNodeRestricted>false</executionNodeRestricted>
            <name>GetFile</name>
            <relationships>
                <autoTerminate>false</autoTerminate>
                <name>success</name>
            </relationships>
            <state>STOPPED</state>
            <style/>
            <type>org.apache.nifi.processors.standard.GetFile</type>
        </processors>
        <processors>
            <id>500e2a2e-6b1f-3aa0-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <position>
                <x>662.798912533494</x>
                <y>287.1745934674873</y>
            </position>
            <bundle>
                <artifact>nifi-avro-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <config>
                <bulletinLevel>WARN</bulletinLevel>
                <comments></comments>
                <concurrentlySchedulableTaskCount>1</concurrentlySchedulableTaskCount>
                <descriptors>
                    <entry>
                        <key>JSON container options</key>
                        <value>
                            <name>JSON container options</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Wrap Single Record</key>
                        <value>
                            <name>Wrap Single Record</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Avro schema</key>
                        <value>
                            <name>Avro schema</name>
                        </value>
                    </entry>
                </descriptors>
                <executionNode>ALL</executionNode>
                <lossTolerant>false</lossTolerant>
                <penaltyDuration>30 sec</penaltyDuration>
                <properties>
                    <entry>
                        <key>JSON container options</key>
                        <value>array</value>
                    </entry>
                    <entry>
                        <key>Wrap Single Record</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>Avro schema</key>
                    </entry>
                </properties>
                <runDurationMillis>0</runDurationMillis>
                <schedulingPeriod>0 sec</schedulingPeriod>
                <schedulingStrategy>TIMER_DRIVEN</schedulingStrategy>
                <yieldDuration>1 sec</yieldDuration>
            </config>
            <executionNodeRestricted>false</executionNodeRestricted>
            <name>ConvertAvroToJSON</name>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>failure</name>
            </relationships>
            <relationships>
                <autoTerminate>false</autoTerminate>
                <name>success</name>
            </relationships>
            <state>STOPPED</state>
            <style/>
            <type>org.apache.nifi.processors.avro.ConvertAvroToJSON</type>
        </processors>
        <processors>
            <id>609c6845-640a-3edc-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <position>
                <x>663.9475498113296</x>
                <y>0.0</y>
            </position>
            <bundle>
                <artifact>nifi-kite-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <config>
                <bulletinLevel>WARN</bulletinLevel>
                <comments></comments>
                <concurrentlySchedulableTaskCount>1</concurrentlySchedulableTaskCount>
                <descriptors>
                    <entry>
                        <key>Hadoop configuration files</key>
                        <value>
                            <name>Hadoop configuration files</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Record schema</key>
                        <value>
                            <name>Record schema</name>
                        </value>
                    </entry>
                    <entry>
                        <key>CSV charset</key>
                        <value>
                            <name>CSV charset</name>
                        </value>
                    </entry>
                    <entry>
                        <key>CSV delimiter</key>
                        <value>
                            <name>CSV delimiter</name>
                        </value>
                    </entry>
                    <entry>
                        <key>CSV quote character</key>
                        <value>
                            <name>CSV quote character</name>
                        </value>
                    </entry>
                    <entry>
                        <key>CSV escape character</key>
                        <value>
                            <name>CSV escape character</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Use CSV header line</key>
                        <value>
                            <name>Use CSV header line</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Lines to skip</key>
                        <value>
                            <name>Lines to skip</name>
                        </value>
                    </entry>
                    <entry>
                        <key>kite-compression-type</key>
                        <value>
                            <name>kite-compression-type</name>
                        </value>
                    </entry>
                </descriptors>
                <executionNode>ALL</executionNode>
                <lossTolerant>false</lossTolerant>
                <penaltyDuration>30 sec</penaltyDuration>
                <properties>
                    <entry>
                        <key>Hadoop configuration files</key>
                    </entry>
                    <entry>
                        <key>Record schema</key>
                        <value>{
     "type": "record",
     "namespace": "example",
     "name": "test",
     "fields": [
   	   { "name": "id", "type": "int" },
	   { "name": "first_name", "type": "string" },
	   { "name": "last_name", "type": "string" },
	   { "name": "company", "type": "string" }
  ]
}</value>
                    </entry>
                    <entry>
                        <key>CSV charset</key>
                        <value>utf8</value>
                    </entry>
                    <entry>
                        <key>CSV delimiter</key>
                        <value>,</value>
                    </entry>
                    <entry>
                        <key>CSV quote character</key>
                        <value>"</value>
                    </entry>
                    <entry>
                        <key>CSV escape character</key>
                        <value>\</value>
                    </entry>
                    <entry>
                        <key>Use CSV header line</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>Lines to skip</key>
                        <value>0</value>
                    </entry>
                    <entry>
                        <key>kite-compression-type</key>
                        <value>SNAPPY</value>
                    </entry>
                </properties>
                <runDurationMillis>0</runDurationMillis>
                <schedulingPeriod>0 sec</schedulingPeriod>
                <schedulingStrategy>TIMER_DRIVEN</schedulingStrategy>
                <yieldDuration>1 sec</yieldDuration>
            </config>
            <executionNodeRestricted>false</executionNodeRestricted>
            <name>ConvertCSVToAvro</name>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>failure</name>
            </relationships>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>incompatible</name>
            </relationships>
            <relationships>
                <autoTerminate>false</autoTerminate>
                <name>success</name>
            </relationships>
            <state>STOPPED</state>
            <style/>
            <type>org.apache.nifi.processors.kite.ConvertCSVToAvro</type>
        </processors>
        <processors>
            <id>a3f416b4-ae35-3d0f-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <position>
                <x>3.4460049181753902</x>
                <y>657.0554716580781</y>
            </position>
            <bundle>
                <artifact>nifi-standard-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <config>
                <bulletinLevel>WARN</bulletinLevel>
                <comments></comments>
                <concurrentlySchedulableTaskCount>1</concurrentlySchedulableTaskCount>
                <descriptors>
                    <entry>
                        <key>JDBC Connection Pool</key>
                        <value>
                            <identifiesControllerService>org.apache.nifi.dbcp.DBCPService</identifiesControllerService>
                            <name>JDBC Connection Pool</name>
                        </value>
                    </entry>
                    <entry>
                        <key>putsql-sql-statement</key>
                        <value>
                            <name>putsql-sql-statement</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Support Fragmented Transactions</key>
                        <value>
                            <name>Support Fragmented Transactions</name>
                        </value>
                    </entry>
                    <entry>
                        <key>database-session-autocommit</key>
                        <value>
                            <name>database-session-autocommit</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Transaction Timeout</key>
                        <value>
                            <name>Transaction Timeout</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Batch Size</key>
                        <value>
                            <name>Batch Size</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Obtain Generated Keys</key>
                        <value>
                            <name>Obtain Generated Keys</name>
                        </value>
                    </entry>
                    <entry>
                        <key>rollback-on-failure</key>
                        <value>
                            <name>rollback-on-failure</name>
                        </value>
                    </entry>
                </descriptors>
                <executionNode>ALL</executionNode>
                <lossTolerant>false</lossTolerant>
                <penaltyDuration>30 sec</penaltyDuration>
                <properties>
                    <entry>
                        <key>JDBC Connection Pool</key>
                        <value>d20851ae-43ce-37d5-0000-000000000000</value>
                    </entry>
                    <entry>
                        <key>putsql-sql-statement</key>
                    </entry>
                    <entry>
                        <key>Support Fragmented Transactions</key>
                        <value>true</value>
                    </entry>
                    <entry>
                        <key>database-session-autocommit</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>Transaction Timeout</key>
                    </entry>
                    <entry>
                        <key>Batch Size</key>
                        <value>100</value>
                    </entry>
                    <entry>
                        <key>Obtain Generated Keys</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>rollback-on-failure</key>
                        <value>false</value>
                    </entry>
                </properties>
                <runDurationMillis>0</runDurationMillis>
                <schedulingPeriod>0 sec</schedulingPeriod>
                <schedulingStrategy>TIMER_DRIVEN</schedulingStrategy>
                <yieldDuration>1 sec</yieldDuration>
            </config>
            <executionNodeRestricted>false</executionNodeRestricted>
            <name>PutSQL</name>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>failure</name>
            </relationships>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>retry</name>
            </relationships>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>success</name>
            </relationships>
            <state>STOPPED</state>
            <style/>
            <type>org.apache.nifi.processors.standard.PutSQL</type>
        </processors>
        <processors>
            <id>b9b34187-d5c4-3730-0000-000000000000</id>
            <parentGroupId>78ed13ea-8e77-355e-0000-000000000000</parentGroupId>
            <position>
                <x>0.0</x>
                <y>279.13366432551345</y>
            </position>
            <bundle>
                <artifact>nifi-standard-nar</artifact>
                <group>org.apache.nifi</group>
                <version>1.9.2</version>
            </bundle>
            <config>
                <bulletinLevel>WARN</bulletinLevel>
                <comments></comments>
                <concurrentlySchedulableTaskCount>1</concurrentlySchedulableTaskCount>
                <descriptors>
                    <entry>
                        <key>JDBC Connection Pool</key>
                        <value>
                            <identifiesControllerService>org.apache.nifi.dbcp.DBCPService</identifiesControllerService>
                            <name>JDBC Connection Pool</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Statement Type</key>
                        <value>
                            <name>Statement Type</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Table Name</key>
                        <value>
                            <name>Table Name</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Catalog Name</key>
                        <value>
                            <name>Catalog Name</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Schema Name</key>
                        <value>
                            <name>Schema Name</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Translate Field Names</key>
                        <value>
                            <name>Translate Field Names</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Unmatched Field Behavior</key>
                        <value>
                            <name>Unmatched Field Behavior</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Unmatched Column Behavior</key>
                        <value>
                            <name>Unmatched Column Behavior</name>
                        </value>
                    </entry>
                    <entry>
                        <key>Update Keys</key>
                        <value>
                            <name>Update Keys</name>
                        </value>
                    </entry>
                    <entry>
                        <key>jts-quoted-identifiers</key>
                        <value>
                            <name>jts-quoted-identifiers</name>
                        </value>
                    </entry>
                    <entry>
                        <key>jts-quoted-table-identifiers</key>
                        <value>
                            <name>jts-quoted-table-identifiers</name>
                        </value>
                    </entry>
                    <entry>
                        <key>jts-sql-param-attr-prefix</key>
                        <value>
                            <name>jts-sql-param-attr-prefix</name>
                        </value>
                    </entry>
                    <entry>
                        <key>table-schema-cache-size</key>
                        <value>
                            <name>table-schema-cache-size</name>
                        </value>
                    </entry>
                </descriptors>
                <executionNode>ALL</executionNode>
                <lossTolerant>false</lossTolerant>
                <penaltyDuration>30 sec</penaltyDuration>
                <properties>
                    <entry>
                        <key>JDBC Connection Pool</key>
                        <value>d20851ae-43ce-37d5-0000-000000000000</value>
                    </entry>
                    <entry>
                        <key>Statement Type</key>
                        <value>INSERT</value>
                    </entry>
                    <entry>
                        <key>Table Name</key>
                        <value>test</value>
                    </entry>
                    <entry>
                        <key>Catalog Name</key>
                    </entry>
                    <entry>
                        <key>Schema Name</key>
                    </entry>
                    <entry>
                        <key>Translate Field Names</key>
                        <value>true</value>
                    </entry>
                    <entry>
                        <key>Unmatched Field Behavior</key>
                        <value>Ignore Unmatched Fields</value>
                    </entry>
                    <entry>
                        <key>Unmatched Column Behavior</key>
                        <value>Fail on Unmatched Columns</value>
                    </entry>
                    <entry>
                        <key>Update Keys</key>
                    </entry>
                    <entry>
                        <key>jts-quoted-identifiers</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>jts-quoted-table-identifiers</key>
                        <value>false</value>
                    </entry>
                    <entry>
                        <key>jts-sql-param-attr-prefix</key>
                        <value>sql</value>
                    </entry>
                    <entry>
                        <key>table-schema-cache-size</key>
                        <value>100</value>
                    </entry>
                </properties>
                <runDurationMillis>0</runDurationMillis>
                <schedulingPeriod>0 sec</schedulingPeriod>
                <schedulingStrategy>TIMER_DRIVEN</schedulingStrategy>
                <yieldDuration>1 sec</yieldDuration>
            </config>
            <executionNodeRestricted>false</executionNodeRestricted>
            <name>ConvertJSONToSQL</name>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>failure</name>
            </relationships>
            <relationships>
                <autoTerminate>true</autoTerminate>
                <name>original</name>
            </relationships>
            <relationships>
                <autoTerminate>false</autoTerminate>
                <name>sql</name>
            </relationships>
            <state>STOPPED</state>
            <style/>
            <type>org.apache.nifi.processors.standard.ConvertJSONToSQL</type>
        </processors>
    </snippet>
    <timestamp>10/21/2019 12:44:33 CEST</timestamp>
</template>
