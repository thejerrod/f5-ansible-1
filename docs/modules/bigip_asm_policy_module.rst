.. _bigip_asm_policy:


bigip_asm_policy - Manage BIG-IP ASM policies
+++++++++++++++++++++++++++++++++++++++++++++

.. versionadded:: 2.5


.. contents::
   :local:
   :depth: 2


Synopsis
--------

* Manage BIG-IP ASM policies.


Requirements (on host that executes module)
-------------------------------------------

  * f5-sdk >= 3.0.4


Options
-------

.. raw:: html

    <table border=1 cellpadding=4>
    <tr>
    <th class="head">parameter</th>
    <th class="head">required</th>
    <th class="head">default</th>
    <th class="head">choices</th>
    <th class="head">comments</th>
    </tr>
                <tr><td>active<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td></td>
        <td><ul><li>True</li><li>False</li></ul></td>
        <td><div>If <code>yes</code> will apply and activate existing inactive policy. If <code>no</code>, it will deactivate existing active policy. Generally should be <code>yes</code> only in cases where you want to activate new or existing policy.</div>        </td></tr>
                <tr><td>file<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td></td>
        <td></td>
        <td><div>Full path to a policy file to be imported into the BIG-IP ASM.</div><div>Policy files exported from newer versions of BIG-IP cannot be imported into older versions of BIG-IP. The opposite, however, is true; you can import older into newer.</div>        </td></tr>
                <tr><td>name<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td></td>
        <td></td>
        <td><div>The ASM policy to manage or create.</div>        </td></tr>
                <tr><td>partition<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td>Common</td>
        <td></td>
        <td><div>Device partition to manage resources on.</div>        </td></tr>
                <tr><td>state<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td></td>
        <td><ul><li>present</li><li>absent</li></ul></td>
        <td><div>When <code>state</code> is <code>present</code>, and <code>file</code> or <code>template</code> parameter is provided, new ASM policy is imported and created with the given <code>name</code>.</div><div>When <code>state</code> is present and no <code>file</code> or <code>template</code> parameter is provided new blank ASM policy is created with the given <code>name</code>.</div><div>When <code>state</code> is <code>absent</code>, ensures that the policy is removed, even if it is currently active.</div>        </td></tr>
                <tr><td>template<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td></td>
        <td><ul><li>ActiveSync v1.0 v2.0 (http)</li><li>ActiveSync v1.0 v2.0 (https)</li><li>Comprehensive</li><li>Drupal</li><li>Fundamental</li><li>Joomla</li><li>LotusDomino 6.5 (http)</li><li>LotusDomino 6.5 (https)</li><li>OWA Exchange 2003 (http)</li><li>OWA Exchange 2003 (https)</li><li>OWA Exchange 2003 with ActiveSync (http)</li><li>OWA Exchange 2003 with ActiveSync (https)</li><li>OWA Exchange 2007 (http)</li><li>OWA Exchange 2007 (https)</li><li>OWA Exchange 2007 with ActiveSync (http)</li><li>OWA Exchange 2007 with ActiveSync (https)</li><li>OWA Exchange 2010 (http)</li><li>OWA Exchange 2010 (https)</li><li>Oracle 10g Portal (http)</li><li>Oracle 10g Portal (https)</li><li>Oracle Applications 11i (http)</li><li>Oracle Applications 11i (https)</li><li>PeopleSoft Portal 9 (http)</li><li>PeopleSoft Portal 9 (https)</li><li>Rapid Deployment Policy</li><li>SAP NetWeaver 7 (http)</li><li>SAP NetWeaver 7 (https)</li><li>SharePoint 2003 (http)</li><li>SharePoint 2003 (https)</li><li>SharePoint 2007 (http)</li><li>SharePoint 2007 (https)</li><li>SharePoint 2010 (http)</li><li>SharePoint 2010 (https)</li><li>Vulnerability Assessment Baseline</li><li>Wordpress</li></ul></td>
        <td><div>An ASM policy built-in template. If the template does not exist we will raise an error.</div><div>Once the policy has been created, this value cannot change.</div><div>The <code>Comprehensive</code>, <code>Drupal</code>, <code>Fundamental</code>, <code>Joomla</code>, <code>Vulnerability Assessment Baseline</code>, and <code>Wordpress</code> templates are only available on BIG-IP versions &gt;= 13.</div>        </td></tr>
        </table>
    </br>



Examples
--------

 ::

    
    - name: Import and activate ASM policy
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_asm_policy
        file: /root/asm_policy.xml
        active: yes
        state: present
      delegate_to: localhost

    - name: Import ASM policy from template
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_sharepoint_policy
        template: SharePoint 2007 (http)
        state: present
      delegate_to: localhost

    - name: Create blank ASM policy
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_blank_policy
        state: present
      delegate_to: localhost

    - name: Create blank ASM policy and activate
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_blank_policy
        active: yes
        state: present
      delegate_to: localhost

    - name: Activate ASM policy
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: inactive_policy
        active: yes
        state: present
      delegate_to: localhost

    - name: Deactivate ASM policy
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: active_policy
        state: present
      delegate_to: localhost

    - name: Import and activate ASM policy in Role
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_asm_policy
        file: "{{ role_path }}/files/asm_policy.xml"
        active: yes
        state: present
      delegate_to: localhost

    - name: Import ASM binary policy
      bigip_asm_policy:
        server: lb.mydomain.com
        user: admin
        password: secret
        name: new_asm_policy
        file: "/root/asm_policy.plc"
        active: yes
        state: present
      delegate_to: localhost


Return Values
-------------

Common return values are :doc:`documented here <http://docs.ansible.com/ansible/latest/common_return_values.html>`, the following are the fields unique to this module:

.. raw:: html

    <table border=1 cellpadding=4>
    <tr>
    <th class="head">name</th>
    <th class="head">description</th>
    <th class="head">returned</th>
    <th class="head">type</th>
    <th class="head">sample</th>
    </tr>

        <tr>
        <td> active </td>
        <td> Set when activating/deactivating ASM policy </td>
        <td align=center> changed </td>
        <td align=center> bool </td>
        <td align=center> True </td>
    </tr>
            <tr>
        <td> state </td>
        <td> Action performed on the target device. </td>
        <td align=center> changed </td>
        <td align=center> string </td>
        <td align=center> absent </td>
    </tr>
            <tr>
        <td> file </td>
        <td> Local path to ASM policy file. </td>
        <td align=center> changed </td>
        <td align=center> string </td>
        <td align=center> /root/some_policy.xml </td>
    </tr>
            <tr>
        <td> template </td>
        <td> Name of the built-in ASM policy template </td>
        <td align=center> changed </td>
        <td align=center> string </td>
        <td align=center> OWA Exchange 2007 (https) </td>
    </tr>
            <tr>
        <td> name </td>
        <td> Name of the ASM policy to be managed/created </td>
        <td align=center> changed </td>
        <td align=center> string </td>
        <td align=center> Asm_APP1_Transparent </td>
    </tr>
        
    </table>
    </br></br>

Notes
-----

.. note::
    - For more information on using Ansible to manage F5 Networks devices see https://www.ansible.com/ansible-f5.



Status
~~~~~~

This module is flagged as **preview** which means that it is not guaranteed to have a backwards compatible interface.


Support
~~~~~~~

This module is community maintained without core committer oversight.

For more information on what this means please read :doc:`/usage/support`


For help developing modules, should you be so inclined, please read :doc:`Getting Involved </development/getting-involved>`, :doc:`Writing a Module </development/writing-a-module>` and :doc:`Guidelines </development/guidelines>`.