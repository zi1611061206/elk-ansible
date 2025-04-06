# Grouping Tasks in Ansible: When to Use Tasks, Blocks, and Separate Files

Ansible is a powerful automation tool that lets you orchestrate complex infrastructures with clear, human‐readable playbooks. One of the key design decisions when writing playbooks is how to group related tasks. You can simply list tasks in a playbook, group them together in a block, or split them into separate files for reuse and better organization. In this article, we discuss when each method is most appropriate.

---

## 1. Using a Simple List of Tasks

When your automation is relatively straightforward and the number of tasks is small, you can define all tasks in a single playbook file. This approach is simple and works well if:

- **The tasks are tightly coupled:** They perform sequential actions with little need for special error handling.
- **No shared directives are needed:** There’s no need to apply common conditions, privilege escalation, or error handling to a group of tasks.

A flat task list is easy to write and understand for small or one-off plays. However, as your playbook grows, you may notice that readability and maintainability start to suffer.

---

## 2. Grouping Tasks with Blocks

Ansible’s `block` feature allows you to logically group a set of tasks. Blocks are especially useful when you need to:

- **Apply common directives:** For example, if every task in the group should run with elevated privileges (`become`), share a common `when` condition, or have a specific tag.
- **Implement error handling:** Blocks support `rescue` and `always` sections, providing a built-in mechanism for handling failures and cleanup operations (similar to try/catch/finally in programming languages).

For example, consider this block that installs a package and then starts a service. If any task fails, the rescue section runs:

```yaml
- hosts: webservers
  tasks:
    - block:
      - name: Install Apache package
        ansible.builtin.yum:
          name: httpd
          state: present

      - name: Start Apache service
        ansible.builtin.service:
          name: httpd
          state: started
    - rescue:
      - name: Log failure and notify administrator
        ansible.builtin.debug:
          msg: "Installation failed, please check the logs."
    - always:
      - name: Cleanup temporary files
        ansible.builtin.file:
          path: /tmp/apache-install
          state: absent
```

In the example above, any condition or privilege directive (like `when` or `become`) applied to the block is inherited by all tasks inside.

---

## 3. Splitting Tasks into Separate Files

As playbooks grow in size or complexity, grouping tasks into separate files becomes a valuable practice. Splitting tasks into different files is recommended when:

- **Reusability is desired:** If a group of tasks is used in several plays or across multiple roles, putting them in their own file (or even in a dedicated role) avoids duplication.
- **Improved organization:** A very long playbook can be difficult to navigate. Dividing tasks logically into multiple files improves readability and maintainability.
- **Separation of concerns:** Different sets of tasks (e.g., installing dependencies versus configuring the application) benefit from being isolated. This allows you to update one set without affecting others.

You can include these separate task files using the `include_tasks` or `import_tasks` directives. For example:

```yaml
- hosts: app_servers
  tasks:
    - name: Include dependency tasks
      ansible.builtin.include_tasks: dependencies.yml

    - name: Include configuration tasks
      ansible.builtin.import_tasks: configuration.yml

    - name: Include cleanup tasks
      ansible.builtin.include_tasks: cleanup.yml
```

> **Note:**
> The difference between `include_tasks` (dynamic inclusion) and `import_tasks` (static inclusion) affects when and how the tasks are loaded and processed.

Splitting tasks into separate files is especially useful for large projects and roles, where each component (e.g., installing packages, setting up users, configuring services) is maintained independently. It also aligns well with the modular design encouraged by Ansible Galaxy and role-based structures shared by the community.

---

## Choosing the Right Approach

- **For small, linear plays:** Use a single task list. This is quick and straightforward.
- **When you need shared conditions, privileges, or error handling:** Group tasks within a block. Blocks let you enforce common directives and provide rescue functionality.
- **For larger, reusable, or logically distinct task groups:** Split tasks into separate files and include them. This approach enhances modularity and maintainability.

In many real-world scenarios, you might combine these techniques. For example, a role might contain its own `tasks/main.yml` file that further uses blocks for error handling. When tasks are complex or reused across different plays, separating them into distinct files (or roles) is the best practice.

---

## Conclusion

Deciding how to group tasks in Ansible depends largely on the complexity and reusability of your automation:
- **Simple lists** work for small jobs.
- **Blocks** offer a way to manage common directives and error handling.
- **Separate files** help maintain large, modular projects.

By choosing the right approach, you can keep your playbooks organized, readable, and maintainable.

---

## References

- [Blocks — Ansible Community Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_blocks.html)
- [Including and Importing Tasks — Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_includes.html)
- [Understanding the Difference Between include and import in Ansible](https://medium.com/@kuldeepkumawat195/understanding-the-difference-between-include-and-import-in-ansible-d7bb0e74a8f3)
- [Blocks with when and include_tasks — Server Fault](https://serverfault.com/questions/1092362/blocks-with-when-and-include-tasks)
- [When do YOU decide to make something a role vs a playbook with ...](https://www.reddit.com/r/ansible/comments/fpemni/when_do_you_decide_to_make_something_a_role_vs_a/)