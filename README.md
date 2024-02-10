## Others

# Convert the Excel file into CSV format using Python code

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

# Change the column type in PostgreSQL database

	ALTER TABLE table_name ALTER COLUMN column_name SET DATA TYPE timestamp USING column_name::timestamp without time zone

# Connector between MSSQL and PostgreSQL databases

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
        
# Connector between IBM Informix and PostgreSQL databases

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
