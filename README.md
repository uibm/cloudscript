# CloudScript

**CloudScript** is a modern, user-friendly web tool for generating cloud-init user data scripts for various operating systems, including Ubuntu, CentOS, Debian, Amazon Linux, RHEL, SUSE, and Windows. It simplifies the process of creating cloud-init configurations for cloud platforms like IBM Cloud, AWS, Azure, and GCP, with support for package installation, user setup, network configuration, storage setup, and custom scripts.

## Features

- **Multi-OS Support**: Generate cloud-init scripts for Ubuntu, CentOS, Debian, Amazon Linux, RHEL, SUSE, and Windows.
- **Customizable Configurations**:
  - Select common packages (e.g., Nginx, Apache, Docker) with OS-specific mappings.
  - Add custom packages via comma-separated input.
  - Configure users with SSH keys and sudo access.
  - Set up network proxies (HTTP, HTTPS, no-proxy).
  - Define storage mounts with device names and file systems.
  - Include custom shell scripts (bash for Linux, PowerShell for Windows).
- **OS-Specific Optimizations**: Automatically includes EPEL for CentOS/RHEL when needed (e.g., for Nginx, Docker) and handles Amazon Linux Docker installation via `amazon-linux-extras`.

## Demo

Try CloudScript live at [uibm.github.io/cloudscript](https://uibm.github.io/cloudscript) 

## Usage

1. **Select an Operating System**:
   - Choose from Ubuntu, CentOS, Debian, Amazon Linux, RHEL, SUSE, or Windows using the OS selector.

2. **Configure Options**:
   - **Packages**: Check common packages (e.g., Nginx, Docker) or enter custom packages (comma-separated).
   - **Users & Security**: Add a username, SSH public key, and choose sudo access (none, password, or no-password).
   - **Network**: Set a hostname and proxy settings (HTTP, HTTPS, no-proxy).
   - **Storage**: Specify mount point, device name, and file system (ext4, xfs, NTFS).
   - **Scripts**: Add custom shell commands (bash for Linux, PowerShell for Windows).

3. **Generate User Data**:
   - Click the "Generate User Data" button to create the cloud-init script.
   - The script is displayed in the output area with proper YAML formatting.

4. **Copy or Download**:
   - Use "Copy to Clipboard" to copy the script with preserved line breaks.
   - Use "Download" to save the script as `userdata-<os>.yml` with correct line endings (LF for Linux, CRLF for Windows).

5. **Use in Cloud Platforms**:
   - Paste the generated script into the user data field when launching instances on IBM Cloud, AWS EC2, Azure, or GCP.
   - For Windows, the script uses `cloudbase-init` compatible YAML for `cloudbase-init` processing.

### Example Output (CentOS with Nginx)

```yaml
#cloud-config
# Generated for CentOS

bootcmd:
  - yum install -y epel-release
package_update: true
packages:
  - nginx
ntp:
  enabled: true
runcmd:
  - echo "Welcome to $(hostname)" > /etc/motd
  - systemctl enable --now firewalld
  - firewall-cmd --permanent --add-service=ssh && firewall-cmd --reload
  - systemctl enable --now chronyd || true
final_message: "cloud-init done."
```

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository: `https://github.com/uibm/cloudscript`
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a pull request with a detailed description.

Please ensure your code follows the existing style and includes tests if applicable.

## Issues

If you encounter issues (e.g., line break problems, browser compatibility, or OS-specific bugs), please file an issue at [github.com/uibm/cloudscript/issues](https://github.com/uibm/cloudscript/issues). Include:
- Browser and version
- Operating system
- Steps to reproduce
- Expected vs. actual behavior

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Icons by [Font Awesome](https://fontawesome.com) (CSS under MIT, icons under CC BY 4.0, icon font under SIL OFL 1.1), delivered via [cdnjs](https://cdnjs.com/) (Cloudflare).
- Typography uses the [Inter](https://fonts.google.com/specimen/Inter) typeface by Rasmus Andersson (SIL Open Font License 1.1), delivered via [Google Fonts](https://fonts.google.com/).
- Generated scripts target [cloud-init](https://cloudinit.readthedocs.io/) for Linux and [cloudbase-init](https://cloudbase-init.readthedocs.io/) for Windows.
- Windows package installs use [Chocolatey](https://chocolatey.org/).
- Red Hat family configurations automatically enable [EPEL](https://docs.fedoraproject.org/en-US/epel/) when needed, and Amazon Linux uses [`amazon-linux-extras`](https://docs.aws.amazon.com/linux/al2/ug/amazon-linux-extras.html) for Docker.

## Contact

For questions or feedback, reach out via GitHub issues or contact [uibm](https://github.com/uibm).