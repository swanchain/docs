# SWAN Orchestrator SDK - Function and Parameter Reference

## 1. Get Instance Resources

### Method: `swan_orchestrator.get_instance_resources(**kwargs)`

Retrieves a list of instance resources (available or all). Provides a comprehensive reference for instance
configurations.
[Function Documentation](https://github.com/swanchain/python-sdk-docs-samples/blob/main/compute/get_instance_resources.py)

**Request Syntax:**

```python
response = swan_orchestrator.get_instance_resources()
```

**Parameters:**

| Parameter   | Type    | Required | Description                                                         | Default |
|-------------|---------|----------|---------------------------------------------------------------------|---------|
| `available` | Boolean | No       | Indicates whether to show only available resources or all resources | `True`  |

**Usage Notes:**

- When `available` is `True`, returns only available resources
- When `available` is `False`, returns all resource configurations

## 2. Create Task

### Method: `swan_orchestrator.create_task(**kwargs)`

Creates a task on SWAN orchestrator with flexible deployment options.
[Function Documentation](https://github.com/swanchain/python-sdk-docs-samples/blob/main/compute/create_task.py)

**Parameters:**

| Parameter           | Type    | Required | Description                                                              | Default        |
|---------------------|---------|----------|--------------------------------------------------------------------------|----------------|
| `wallet_address`    | String  | Yes      | Wallet address associated with the newly created task                    | -              |
| `instance_type`     | String  | No       | Instance type of hardware configuration                                  | `'C1ae.small'` |
| `region`            | String  | No       | Region of hardware                                                       | `global`       |
| `duration`          | Integer | No       | Service runtime duration in seconds                                      | 3600 (1 hour)  |
| `app_repo_image`    | String  | No*      | Demo space name. Automatically sets `auto_pay` to `True` if used         | -              |
| `job_source_uri`    | String  | No*      | Job source URI for deployment. Overrides `app_repo_image` and `repo_uri` | -              |
| `repo_uri`          | String  | No*      | Repository URI to be deployed                                            | -              |
| `repo_branch`       | String  | No       | Repository branch to be deployed                                         | -              |
| `auto_pay`          | Boolean | No       | Automatically pays to deploy task                                        | `True`         |
| `private_key`       | String  | No**     | Wallet's private key                                                     | -              |
| `preferred_cp_list` | List    | No       | List of preferred CP account addresses                                   | -              |
| `ip_whitelist`      | List    | No       | List of IP addresses allowed to access the application                   | -              |

**Deployment Priority:**

1. `job_source_uri` (Highest priority)
2. `app_repo_image`
3. `repo_uri`

**Notes:**

- At least one of `job_source_uri`, `app_repo_image`, or `repo_uri` must be provided
- If `auto_pay` is `True`, task deployment is automatic
- If `auto_pay` is `False`, manual payment confirmation is required

## 3. Get Deployment Information

### Method: `swan_orchestrator.get_deployment_info(**kwargs)`

Retrieves deployment information for a specific task.
[Function Documentation](https://github.com/swanchain/python-sdk-docs-samples/blob/main/compute/get_task_deployment_info.py)

**Request Syntax:**

```python
response = swan_orchestrator.get_deployment_info(task_uuid="string")
```

**Parameters:**

| Parameter   | Type   | Required | Description                   | Default |
|-------------|--------|----------|-------------------------------|---------|
| `task_uuid` | String | Yes      | Unique identifier of the task | -       |

## 4. Get Real URL

### Method: `swan_orchestrator.get_real_url(**kwargs)`

Retrieves the real URL for a specific task.

**Request Syntax:**

```python
response = swan_orchestrator.get_real_url(task_uuid="string")
```

**Parameters:**

| Parameter   | Type   | Required | Description                   | Default |
|-------------|--------|----------|-------------------------------|---------|
| `task_uuid` | String | Yes      | Unique identifier of the task | -       |

## 5. Renew Task

### Method: `swan_orchestrator.renew_task(**kwargs)`

Extends the duration of an existing task.
[Function Documentation](https://github.com/swanchain/python-sdk-docs-samples/blob/main/compute/renew_task.py)

**Parameters:**

| Parameter     | Type    | Required | Description                                             | Default |
|---------------|---------|----------|---------------------------------------------------------|---------|
| `task_uuid`   | String  | Yes      | Unique identifier of the task to extend                 | -       |
| `duration`    | Integer | No       | Extension duration in seconds                           | 0       |
| `tx_hash`     | String  | No*      | Transaction hash of payment                             | -       |
| `auto_pay`    | Boolean | No       | Automatically pays to extend task                       | `True`  |
| `private_key` | String  | No**     | Wallet's private key (required if `auto_pay` is `True`) | -       |

**Important Notes:**

- If `auto_pay` is `False`, `tx_hash` must be provided
- If `auto_pay` is `True`, `private_key` must be provided

## 6. Terminate Task

### Method: `swan_orchestrator.terminate_task(**kwargs)`

Terminates a task and provides a refund based on remaining time.
[Function Documentation](https://github.com/swanchain/python-sdk-docs-samples/blob/main/compute/terminate_task.py)

**Request Syntax:**

```python
response = swan_orchestrator.terminate_task(task_uuid="string")
```

**Parameters:**

| Parameter   | Type   | Required | Description                   | Default |
|-------------|--------|----------|-------------------------------|---------|
| `task_uuid` | String | Yes      | Unique identifier of the task | -       |