# Comparing Fully Qualified Collection Names (FQCN) and Short Names in Ansible

Ansible offers two methods for invoking modules: using Fully Qualified Collection Names (FQCN) and using short names. This article explores the differences between these two approaches, outlines their respective advantages and disadvantages, and provides recommendations on when to use each method in professional environments.

## 1. Fully Qualified Collection Names (FQCN)

### Definition
FQCN includes the namespace, collection, and module name. Examples include:
- `community.docker.docker_container`
- `ansible.builtin.blockinfile`

### Advantages
- **Clarity and Unambiguity:**  
  Specifying the namespace and collection explicitly ensures that both Ansible and users know exactly which module is being referenced. This is particularly important when multiple modules share the same name across different collections.

- **Ease of Maintenance:**  
  In complex projects with numerous dependencies, FQCN guarantees that the playbook consistently calls the correct module. This minimizes the risk of conflicts and simplifies long-term maintenance.

- **Alignment with Modern Best Practices:**  
  Recent versions of Ansible encourage the use of FQCN to enhance consistency and clarity in module usage across different environments.  
  :contentReference[oaicite:0]{index=0}

## 2. Short Names

### Definition
Short names reference the module without including the namespace or collection. Examples include:
- `docker_container`
- `blockinfile`

### Advantages
- **Convenience and Brevity:**  
  Short names reduce the length of playbooks, making them easier to write and read in simple or exploratory scenarios.

- **Sufficiency for Built-in Modules:**  
  When using modules from `ansible.builtin`, short names are generally unambiguous, as these modules are recognized by default in Ansible.  
  :contentReference[oaicite:1]{index=1}

## 3. When to Use Each Approach

- **Use FQCN When:**
    - Working with modules from external collections.
    - There is a potential for conflicts due to modules with the same name from different sources.
    - Managing professional or large-scale projects where clarity, consistency, and ease of maintenance are critical.

- **Use Short Names When:**
    - Using only built-in modules where the risk of ambiguity is minimal.
    - Creating simple examples or during rapid development, where brevity is more important than detailed namespace specification.

## 4. General Best Practices

For professional environments—especially in production or complex projects—it is advisable to standardize on FQCN. This practice:
- Ensures unambiguous module references.
- Reduces the risk of name conflicts.
- Enhances overall maintainability and clarity of playbooks.

In contrast, short names can be acceptable for small-scale projects or quick experiments where only built-in modules are involved and clarity is less of a concern.

## 5. Conclusion

While short names offer convenience and a cleaner syntax in simple scenarios, the use of Fully Qualified Collection Names (FQCN) is recommended for professional projects. FQCN minimizes ambiguity, prevents potential module conflicts, and promotes robust, maintainable playbooks—key factors for long-term project success.

## References

- [Ansible Collections Documentation](https://docs.ansible.com/ansible/latest/reference_appendices/collections.html)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
