# Azure Repos Export

## Get Repos:

```csharp


$org="https://dev.azure.com/SOME_PROJECT"
#az extension add --name azure-devops

az devops configure --defaults organization=$org

$total_project_count = 0
$total_repo_count = 0
$total_repo_size = 0
$item = 0

write-host "Getting Projects..." -foregroundcolor yellow

#az devops project list --query "value[].description" -o tsv
$projects = (az devops project list --top 1000 | convertfrom-json).value
$total_project_count = $projects.count

$json = @{}
$value = New-Object System.Collections.ArrayList

$projects | %  {
    $item += 1
    $repo_num = 0
    $repo_size = 0

    $proj_id = $_.id
    $proj_name = $_.name
    $proj_desc = $_.description
    
    $per_done = [math]::Round(($item / $total_project_count),2)*100

    #write-host "`nProject name: $proj_name" -foregroundcolor yellow

    $repos = az repos list --project $_.id | convertfrom-json

    Write-Progress -Activity "Searching Projects" -Status "$item / $total_project_count Projects, $total_repo_count Repos, $total_repo_size Size" -PercentComplete $per_done -CurrentOperation Projects
    $i = 0
    $repo_num = $repos.count

    $repos | % {
        $i += 1
        $repo_size = $repo_size + $_.size
        $per_repo = [math]::Round(($i / $repo_num),2)*100

        $obj = @{
            project_id = $proj_id;
            project_name = $proj_name;
            project_desc = $proj_desc;
            name = $_.name;
            url = $_.remoteUrl;
            size = $_.size;
        }
        $value.Add($obj) | out-null


        Write-Progress -Id 1 -Activity "Searching Repos" -Status "$item / $total_project_count" -PercentComplete $per_repo -CurrentOperation Repos
    }

    $total_repo_count += $repos.count
    $total_repo_size += $repo_size
    

    # write-host "Repo Size  :==> $repo_size" -foregroundcolor magenta
    # write-host "Repo Count :==> $repo_num" -foregroundcolor magenta

    # write-host "-======="
    # write-host "Progress            :==> $item / $total_project_count ($per_done %)" -foregroundcolor green
    # write-host "Total Repo Count    :==> $total_repo_count" -foregroundcolor yellow
    # write-host "Total Repo Size     :==> $total_repo_size" -foregroundcolor yellow
    # write-host "-======================================="
}
    $json.Add("value",$value) | out-null

    $file = "ado-repos.json"
    $json | convertto-json | sort-object $project_name | out-file $file


write-host "Total Project Count :==> $total_project_count" -foregroundcolor green
write-host "Total Repo Count    :==> $total_repo_count" -foregroundcolor green
write-host "Total Repo Size     :==> $total_repo_size" -foregroundcolor green



# -=======================================
# Total Project Count :==> 122
# Total Repo Count    :==> 1776
# Total Repo Size     :==> 69074252494
# Total Project Count :==> 122
# Total Repo Count    :==> 1776
# Total Repo Size     :==> 69074252494



$total_repo_size = 69074252494

$result = $total_repo_size/1Gb
```

