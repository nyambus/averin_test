Триггеры:

`Monitoring - Hosts - Zabbix server - Triggers`
    
    Create trigger
    
      Name: example
      
      Expression - Add

        Item - Select - CPU Utilization 

        Function: avg()
        
        Last of (T): 1m - Time

        Result > 0 

        Insert
    
