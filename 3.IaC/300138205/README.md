

* terraform.tfvars

```lua
pm_vm_name      = "vm138205"   
pm_ipconfig0    = "ip=10.7.237.196/23,gw=10.7.237.1"
pm_nameserver   = "10.7.237.3"    
pm_url          = "https://10.7.237.16:8006/api2/json"
pm_token_id     = "tofu@pve!opentofu"
pm_token_secret = "1cde2cfc-e100-47b9-9ee2-591ed83cfb8e"
sshkeys = [
  file("~/.ssh/taylor.pub"),
  file("~/.ssh/b300098957@ramena.pub")
]
```



