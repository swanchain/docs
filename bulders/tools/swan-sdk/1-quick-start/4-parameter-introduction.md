# Parameter introduction

## create_task

| Parameter           | Type    | Required | Description                                                                             | Default               |
|---------------------|---------|----------|-----------------------------------------------------------------------------------------|-----------------------|
| `wallet_address`    | string  | YES      | The wallet address to be associated with newly created task                             | -                     |
| `instance_type`     | string  | NO       | Instance type of hardware config                                                        | 'C1ae.small'          |
| `region`            | string  | NO       | Region of hardware                                                                      | global                |
| `duration`          | integer | NO       | Duration of service runtime in seconds                                                  | 3600 seconds (1 hour) |
| `app_repo_image`    | string  | NO       | Name of a demo space. If used, `auto_pay` will be True by default                       | -                     |
| `job_source_uri`    | string  | NO*      | Job source URI to be deployed. If provided, `app_repo_image` and `repo_uri` are ignored | -                     |
| `repo_uri`          | string  | NO*      | URI of the repo to be deployed                                                          | -                     |
| `repo_branch`       | string  | NO       | Branch of the repo to be deployed                                                       | -                     |
| `auto_pay`          | Boolean | NO       | Automatically pays to deploy task                                                       | True                  |
| `private_key`       | string  | NO**     | Wallet's private key                                                                    | -                     |
| `preferred_cp_list` | list    | NO       | List of preferred cp account addresses                                                  | -                     |
| `ip_whitelist`      | list    | NO       | List of IP addresses which can access the application                                   | -                     |

Note: Only one of job_source_uri, app_repo_image, or repo_uri must be provided. Priority is as follows:
1.	job_source_uri (highest priority),
2.	app_repo_image,
3.	repo_uri.