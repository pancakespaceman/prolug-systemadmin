<div class="flex-container">
        <img src="https://github.com/ProfessionalLinuxUsersGroup/img/blob/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true" width="64" height="64"></img>
    <p>
        <h1>Unit 14 Bonus Lab - Ansible + Automation Challenge
</h1>
    </p>
</div>

## Objective
Expand your Ansible skills by creating a self-documenting automation system that:
- Collects system information from multiple servers
- Stores the results in timestamped reports
- Sends the output via a webhook to simulate automation integration with external systems (like Slack, Discord, or CI/CD pipelines)

---

## Part 1: System Info Playbook

Create a new file `collect_info.yaml` with the following content:

```yaml
---
- name: Collect system info and log results
  hosts: servers
  gather_facts: yes
  vars:
    outdir: /tmp/ansible_reports
  tasks:
    - name: Ensure output directory exists
      file:
        path: "{{ outdir }}"
        state: directory
        mode: '0755'

    - name: Create system report
      copy:
        content: |
          Hostname: {{ ansible_hostname }}
          IP: {{ ansible_default_ipv4.address }}
          Uptime: {{ ansible_uptime_seconds }} seconds
          Memory: {{ ansible_memtotal_mb }} MB
        dest: "{{ outdir }}/report_{{ inventory_hostname }}_{{ ansible_date_time.iso8601_basic_short }}.txt"
```

Run the playbook with:

```bash
ansible-playbook -i hosts -k collect_info.yaml
```

---

## Part 2: Webhook Integration

Send a short success message to a Discord webhook when the job completes.

Append this task to the end of your `tasks:` list in `collect_info.yaml`:

```yaml
    - name: Notify via webhook
      uri:
        url: https://discord.com/api/webhooks/your_webhook_url
        method: POST
        headers:
          Content-Type: "application/json"
        body_format: json
        body:
          content: "Ansible report job completed for {{ inventory_hostname }} at {{ ansible_date_time.iso8601 }}"
```

Replace `your_webhook_url` with your actual Discord webhook.

---

## Bonus Challenge

- Modify the playbook to install a package (`htop`, `tree`, or `vim`) **only if it’s missing**.
- Schedule the playbook to run nightly using `cron` or a `systemd` timer.
