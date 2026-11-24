# Splunk Role-Based Access Control (RBAC) Investigation

## 1. Investigation Objective

The objective of this exercise was to explore Role-Based Access Control (RBAC) in Splunk Enterprise by creating custom roles, assigning test users, and examining how role inheritance, capabilities, and index permissions affect access to Splunk data.

The exercise compared an analyst role that inherited permissions from a built-in role with a standalone restricted role configured for access to a specific index.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| SIEM Platform | Splunk Enterprise |
| Splunk Version | 10.6.2.5 |
| Operating System | Kali Linux |
| Virtualisation | Oracle VirtualBox |
| Test Account 1 | soc_analyst_test |
| Test Role 1 | soc_analyst |
| Test Account 2 | soc_restricted_test |
| Test Role 2 | soc_analyst_restricted |

## 3. Role 1: soc_analyst

### 3.1 Role Configuration

The first custom role, `soc_analyst`, was created with the built-in `user` role selected as its parent.

The role inherited permissions from the parent role. Splunk displayed 33 inherited capabilities and no additional native capabilities.

The role also inherited access to all non-internal indexes. This provided broad access to searchable data rather than limiting the analyst to a specific index.

### 3.2 Test User

A test account named `soc_analyst_test` was created and assigned the `soc_analyst` role.

The account was used to verify whether the user could access and search the lab's indexed data.

### 3.3 Search and Access Tests

The following searches were performed while logged in as the test user.

**Test 1: Search the main index**

```spl
index=main
```

Result: 249,132 events returned.

**Test 2: Count events by sourcetype**

```spl
index=main | stats count by sourcetype
```

Results:

| Sourcetype | Event Count |
|---|---:|
| WinEventLog:Application | 4,434 |
| WinEventLog:Security | 22,594 |
| WinEventLog:System | 2,376 |
| access_combined_wcookie | 79,064 |
| secure-2 | 80,176 |
| vendor_sales | 60,488 |

**Test 3: Search Windows Security logs**

```spl
index=main sourcetype="WinEventLog:Security"
```

Result: 22,594 events returned.

**Test 4: Search failed logon events**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
```

Result: 8 failed logon events returned during this test.

**Test 5: Search Windows System logs**

```spl
index=main sourcetype="WinEventLog:System"
```

Result: 2,376 events returned.

**Test 6: Search Windows Application logs**

```spl
index=main sourcetype="WinEventLog:Application"
```

Result: 4,434 events returned.

### 3.4 Observations

The test account successfully searched the main index and accessed multiple Windows and web-related sourcetypes.

The role's inherited permissions provided broad access to non-internal indexes.

The user-management option was not visible in the test account's Settings menu during the interface check. This was an observation of the available interface, not a comprehensive test of every administrative permission.

## 4. Role 2: soc_analyst_restricted

### 4.1 Role Configuration

A second custom role, `soc_analyst_restricted`, was created to demonstrate a more limited permission configuration.

Unlike the first role, this role did not inherit from another role.

The role was configured with:

- Search capability.
- Permission to change its own password.
- `main` as its included index.
- `main` as its default index.
- No other indexes selected in the role's index configuration.

The role configuration displayed seven native capabilities and zero inherited capabilities in Splunk.

### 4.2 Test User

A test account named `soc_restricted_test` was created and assigned the `soc_analyst_restricted` role.

The account was used to verify access to the configured index and to inspect the available user-management interface.

### 4.3 Search and Access Tests

**Test 1: Search the main index**

```spl
index=main
```

Result: 249,132 events returned.

**Test 2: Search failed Windows logons**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
```

Result: 8 events returned.

**Test 3: Test vendor_sales search**

```spl
index=vendor_sales
```

Result: 0 events returned.

**Test 4: Test access_combined_wcookie search**

```spl
index=access_combined_wcookie
```

Result: 0 events returned.

### 4.4 Important Test Limitation

The zero-event results for `vendor_sales` and `access_combined_wcookie` do not independently prove that the restricted role was denied access.

The index configuration was inspected separately using the administrative account. It showed that `main` was the only included and default index for `soc_analyst_restricted`, with other indexes unchecked.

An administrative event-count check also showed:

| Index | Event Count |
|---|---:|
| history | 0 |
| main | 249,132 |
| summary | 0 |

Because the other indexes tested did not provide populated data for a meaningful comparison, runtime denial of access to those indexes was not conclusively established.

### 4.5 User-Management Interface Check

While logged in as `soc_restricted_test`, the Users section under All Configurations displayed no items.

This was consistent with the restricted account not having access to the user-management interface. The exercise did not attempt to test every administrative endpoint or permission.

## 5. Comparison of the Two Roles

| Feature | soc_analyst | soc_analyst_restricted |
|---|---|---|
| Parent role | user | None |
| Inherited capabilities | 33 | 0 |
| Native capabilities shown | 0 | 7 |
| Index configuration | All non-internal indexes inherited | main only |
| Test user | soc_analyst_test | soc_restricted_test |
| Main index search | Successful | Successful |
| Security log search | Successful | Successful |

## 6. Security Observations

This exercise demonstrated several important Splunk administration concepts:

1. **Role inheritance:** A custom role can inherit capabilities and index permissions from a parent role.
2. **Least privilege:** Restricting a role's index configuration can reduce the scope of data available to its users.
3. **User and role separation:** Users are assigned roles, while roles define permissions and capabilities.
4. **Access validation:** Testing with separate accounts helps assess how role configurations affect the user experience.
5. **Evidence quality:** A search returning zero events does not automatically prove access was denied. The index's existence, data availability, and permissions must also be considered.

## 7. Screenshots and Evidence

The following screenshots are stored in `screenshots/`:

### First Role: soc_analyst

- `rbac_role_created.png`
- `rbac_role_list_verification.png`
- `rbac_role_inheritance.png`
- `rbac_role_capabilities.png`
- `rbac_role_indexes.png`
- `rbac_role_providers.png`
- `rbac_role_restrictions.png`
- `rbac_role_resources.png`
- `rbac_test_user_assignment.png`
- `rbac_analyst_sourcetype_access.png`
- `rbac_analyst_security_log_access.png`
- `rbac_analyst_failed_logon_search.png`
- `rbac_analyst_system_log_access.png`
- `rbac_analyst_application_log_access.png`
- `rbac_analyst_restricted_user_management.png`

### Second Role: soc_analyst_restricted

- `rbac_restricted_role_created.png`
- `rbac_restricted_test_user_created.png`
- `rbac_restricted_main_index_access.png`
- `rbac_restricted_vendor_sales_access_test.png`
- `rbac_restricted_access_combined_test.png`
- `rbac_restricted_user_management_access.png`

## 8. Conclusion

This exercise provided practical experience creating Splunk roles, configuring role inheritance, assigning users, reviewing capabilities, and examining index permissions.

The comparison demonstrated how inherited permissions can provide broad access, while a standalone role can be configured with a narrower index scope.

The restricted role was successfully configured for the main index, and its user successfully searched that index. However, the available test data did not conclusively establish runtime denial of access to other indexes.

The exercise reinforced the importance of applying least-privilege principles and validating access controls using reliable evidence