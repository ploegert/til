# 6: Setup Terraform Workspace



#### Setup your workspace

To ensure you connect to the right state store in the blob, make sure you are configured to the proper env

```text
# to see what workspaces exist on your station:
terraform workspace list

# to create a new workspace:
terraform workspace new dev

# if you have existing workspaces, and want to set to a specific one:
terraform workspace select dev
```

#### 

