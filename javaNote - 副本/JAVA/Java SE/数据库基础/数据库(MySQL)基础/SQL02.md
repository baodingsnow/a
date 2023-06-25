Update rule_device_alram  SET  name=1 WHERE name=NULL;

SELECT IFNULL(name,'111') as name FROM rule_device_alarm;

DELETE  FROM  rule_device_alarm  WHERE  target='product'