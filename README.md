{#
  templates/alert_email.mjml
  Render pipeline: Jinja2 → MRML → final HTML

  Required context:
    year               str   e.g. "2026"
    generated_at       str   e.g. "16 Jun 2026 · 06:00 UTC"
    next_run           str   e.g. "01 Jul 2026 · 06:00 UTC"
    total_overloads    int
    affected_employees int
    build_version      str   e.g. "4.2.1"
    erp_url            str
    subscriptions_url  str
    support_url        str
    departments        list of:
      name             str
      code             str
      employees        list of:
        name           str
        role           str
        fte            list[float | None]  — 12 values, one per month
#}
<mjml>
  <mj-head>
    <mj-font name="Inter"    href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&amp;display=swap" />
    <mj-font name="DM Mono" href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&amp;display=swap" />
    <mj-attributes>
      <mj-all    font-family="Inter, Arial, sans-serif" font-size="13px" color="#374151" />
      <mj-body   background-color="#f0f0f4" width="900px" />
      <mj-section background-color="#ffffff" padding="0" />
      <mj-column  padding="0" />
    </mj-attributes>
  </mj-head>

  <mj-body background-color="#f0f0f4" width="900px">

    {# ── Top accent bar ── #}
    <mj-section padding="0">
      <mj-column>
        <mj-text padding="0" font-size="1px" line-height="4px">
          <div style="height:4px;background:linear-gradient(90deg,#1a1a2e 0%,#c0392b 100%);line-height:4px;font-size:1px;">&nbsp;</div>
        </mj-text>
      </mj-column>
    </mj-section>

    {# ── Header ── #}
    <mj-section background-color="#ffffff" padding="32px 40px 0">
      <mj-column>
        <mj-text padding="0 0 24px">
          <table width="100%" cellpadding="0" cellspacing="0" style="border-collapse:collapse;">
            <tr>
              <td width="56" valign="top" style="padding-right:16px;">
                <div style="width:40px;height:40px;border-radius:8px;background-color:#fde8e8;border:1px solid #fca5a5;text-align:center;line-height:40px;font-size:20px;">&#9888;</div>
              </td>
              <td valign="top">
                <p style="margin:0 0 4px;font-size:10.5px;color:#6b6b80;letter-spacing:0.08em;text-transform:uppercase;font-family:'DM Mono',monospace;">
                  Automated Alert &middot; Resource Planning
                </p>
                <h1 style="margin:0;font-size:22px;color:#1a1a2e;font-weight:700;line-height:1.2;font-family:Inter,Arial,sans-serif;">
                  RCCP &mdash; FTE Overload for {{ year }}
                </h1>
              </td>
              <td valign="top" align="right" style="padding-left:24px;white-space:nowrap;">
                <div style="display:inline-block;background-color:#fde8e8;border:1px solid #fca5a5;border-radius:4px;padding:4px 10px;margin-bottom:6px;">
                  <span style="font-size:11px;color:#b91c1c;font-family:'DM Mono',monospace;font-weight:600;">
                    &#9679; {{ total_overloads }} Overloaded Cells
                  </span>
                </div>
                <br>
                <span style="font-size:11px;color:#9ca3af;font-family:'DM Mono',monospace;">
                  Generated {{ generated_at }}
                </span>
              </td>
            </tr>
          </table>

          {# Description block #}
          <div style="padding:16px 20px;background-color:#f9f9fb;border:1px solid #e5e7eb;border-left:3px solid #1a1a2e;border-radius:4px;">
            <p style="margin:0 0 12px;font-size:13px;color:#374151;line-height:1.7;">
              This automated Rough-Cut Capacity Planning (RCCP) report identifies employees with
              projected Full-Time Equivalent (FTE) values exceeding
              <strong style="color:#1a1a2e;">1.0</strong> across the {{ year }} planning horizon.
              Overloaded resources require immediate review by department managers to prevent
              schedule slippage and burnout risk.
            </p>
            <table cellpadding="0" cellspacing="0" style="border-collapse:collapse;">
              <tr>
                <td style="padding-right:12px;">
                  <div style="background-color:#fff0e6;border:1px solid #fdba74;border-radius:4px;padding:4px 10px;">
                    <span style="font-size:11px;color:#c2570b;font-family:'DM Mono',monospace;font-weight:500;">
                      {{ affected_employees }} Affected Employees
                    </span>
                  </div>
                </td>
                <td style="padding-right:12px;">
                  <div style="background-color:#f0f0f4;border:1px solid #d1d5db;border-radius:4px;padding:4px 10px;">
                    <span style="font-size:11px;color:#374151;font-family:'DM Mono',monospace;font-weight:500;">
                      {{ departments | length }} Departments Reviewed
                    </span>
                  </div>
                </td>
                <td>
                  <div style="background-color:#e8f5e9;border:1px solid #86efac;border-radius:4px;padding:4px 10px;">
                    <span style="font-size:11px;color:#15803d;font-family:'DM Mono',monospace;font-weight:500;">
                      FY {{ year }} &middot; All 12 Months
                    </span>
                  </div>
                </td>
              </tr>
            </table>
          </div>
        </mj-text>
      </mj-column>
    </mj-section>

    {# ── Legend bar ── #}
    <mj-section background-color="#fafafa" padding="12px 40px"
                 border-top="1px solid #e5e7eb" border-bottom="1px solid #e5e7eb">
      <mj-column>
        <mj-text padding="0">
          <table cellpadding="0" cellspacing="0" style="border-collapse:collapse;">
            <tr>
              <td style="padding-right:24px;">
                <span style="font-size:11px;color:#9ca3af;font-family:'DM Mono',monospace;text-transform:uppercase;letter-spacing:0.06em;">FTE Legend:</span>
              </td>
              <td style="padding-right:20px;">
                <span style="display:inline-block;width:22px;height:14px;border-radius:3px;background-color:#fde8e8;border:1px solid rgba(185,28,28,0.13);vertical-align:middle;"></span>
                <span style="font-size:10.5px;color:#6b6b80;font-family:'DM Mono',monospace;vertical-align:middle;margin-left:6px;">Critical &#8805; 1.30</span>
              </td>
              <td style="padding-right:20px;">
                <span style="display:inline-block;width:22px;height:14px;border-radius:3px;background-color:#fff0e6;border:1px solid rgba(194,87,11,0.13);vertical-align:middle;"></span>
                <span style="font-size:10.5px;color:#6b6b80;font-family:'DM Mono',monospace;vertical-align:middle;margin-left:6px;">High &#8805; 1.10</span>
              </td>
              <td style="padding-right:20px;">
                <span style="display:inline-block;width:22px;height:14px;border-radius:3px;background-color:#fffbeb;border:1px solid rgba(180,83,9,0.13);vertical-align:middle;"></span>
                <span style="font-size:10.5px;color:#6b6b80;font-family:'DM Mono',monospace;vertical-align:middle;margin-left:6px;">Warning &#8805; 1.00</span>
              </td>
              <td>
                <span style="display:inline-block;width:22px;height:14px;border-radius:3px;background-color:#f9f9fb;border:1px solid rgba(55,65,81,0.13);vertical-align:middle;"></span>
                <span style="font-size:10.5px;color:#6b6b80;font-family:'DM Mono',monospace;vertical-align:middle;margin-left:6px;">Normal &lt; 1.00</span>
              </td>
            </tr>
          </table>
        </mj-text>
      </mj-column>
    </mj-section>

    {# ── Department sections ── #}
    {% for dept in departments %}

    {# Count overloaded FTE cells for this department #}
    {% set ns = namespace(overloads=0) %}
    {% for emp in dept.employees %}
      {% for v in emp.fte %}
        {% if v is not none and v > 1.0 %}{% set ns.overloads = ns.overloads + 1 %}{% endif %}
      {% endfor %}
    {% endfor %}

    <mj-section background-color="#ffffff"
                padding="{{ '32px 40px 0' if loop.first else '0 40px 0' }}">
      <mj-column>
        <mj-text padding="0 0 32px">

          {# Department header row #}
          <table width="100%" cellpadding="0" cellspacing="0"
                 style="border-collapse:collapse;border-bottom:2px solid #1a1a2e;padding-bottom:12px;margin-bottom:12px;">
            <tr>
              <td valign="middle">
                <span style="display:inline-block;width:32px;height:32px;border-radius:4px;background-color:#1a1a2e;text-align:center;line-height:32px;font-size:14px;color:#ffffff;vertical-align:middle;">&#9632;</span>
                <span style="font-size:13px;color:#1a1a2e;font-weight:600;font-family:Inter,Arial,sans-serif;vertical-align:middle;margin-left:12px;">{{ dept.name }}</span>
                <span style="margin-left:8px;padding:2px 6px;border-radius:3px;font-size:10px;font-family:'DM Mono',monospace;background-color:#f0f0f4;color:#6b6b80;letter-spacing:0.05em;">{{ dept.code }}</span>
              </td>
              {% if ns.overloads > 0 %}
              <td align="right" valign="middle">
                <div style="display:inline-block;background-color:#fde8e8;border:1px solid #fca5a5;border-radius:4px;padding:4px 10px;">
                  <span style="font-size:11px;color:#b91c1c;font-family:'DM Mono',monospace;font-weight:500;">
                    &#9888; {{ ns.overloads }} overload{{ 's' if ns.overloads != 1 else '' }}
                  </span>
                </div>
              </td>
              {% endif %}
            </tr>
          </table>

          {# FTE data table — 14 columns: employee, role, Jan–Dec #}
          <div style="border-radius:6px;border:1px solid #e5e7eb;overflow:hidden;">
            <table width="100%" cellpadding="0" cellspacing="0"
                   style="border-collapse:collapse;font-family:Inter,Arial,sans-serif;">
              <thead>
                <tr style="background-color:#f9f9fb;">
                  <th align="left"
                      style="padding:10px 16px;font-size:11px;color:#6b6b80;font-weight:600;letter-spacing:0.06em;text-transform:uppercase;border-bottom:1px solid #e5e7eb;min-width:160px;">
                    Employee
                  </th>
                  <th align="left"
                      style="padding:10px 12px;font-size:11px;color:#6b6b80;font-weight:600;letter-spacing:0.06em;text-transform:uppercase;border-bottom:1px solid #e5e7eb;min-width:130px;">
                    Role
                  </th>
                  {% for m in ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'] %}
                  <th align="center"
                      style="padding:10px 0;font-size:10px;color:#6b6b80;font-weight:600;letter-spacing:0.04em;text-transform:uppercase;border-bottom:1px solid #e5e7eb;font-family:'DM Mono',monospace;width:52px;">
                    {{ m }}
                  </th>
                  {% endfor %}
                </tr>
              </thead>
              <tbody>
                {% for emp in dept.employees %}
                <tr style="background-color:{{ '#ffffff' if loop.index0 % 2 == 0 else '#fafafa' }};border-bottom:1px solid #f0f0f4;">
                  <td style="padding:10px 16px;font-size:12.5px;color:#1a1a2e;font-weight:500;">{{ emp.name }}</td>
                  <td style="padding:10px 12px;font-size:11.5px;color:#6b6b80;">{{ emp.role }}</td>
                  {% for val in emp.fte %}
                  {%- if val is none -%}
                  <td align="center" style="padding:8px 0;font-size:11.5px;font-family:'DM Mono',monospace;color:#374151;">&mdash;</td>
                  {%- elif val >= 1.3 -%}
                  <td align="center" style="padding:8px 0;font-size:11.5px;font-family:'DM Mono',monospace;font-weight:600;background-color:#fde8e8;color:#b91c1c;border-left:2px solid #ef4444;">{{ "%.2f" | format(val) }}</td>
                  {%- elif val >= 1.1 -%}
                  <td align="center" style="padding:8px 0;font-size:11.5px;font-family:'DM Mono',monospace;font-weight:600;background-color:#fff0e6;color:#c2570b;border-left:2px solid #f97316;">{{ "%.2f" | format(val) }}</td>
                  {%- elif val >= 1.0 -%}
                  <td align="center" style="padding:8px 0;font-size:11.5px;font-family:'DM Mono',monospace;font-weight:600;background-color:#fffbeb;color:#b45309;border-left:2px solid #f59e0b;">{{ "%.2f" | format(val) }}</td>
                  {%- else -%}
                  <td align="center" style="padding:8px 0;font-size:11.5px;font-family:'DM Mono',monospace;color:#374151;">{{ "%.2f" | format(val) }}</td>
                  {%- endif -%}
                  {% endfor %}
                </tr>
                {% endfor %}
              </tbody>
            </table>
          </div>

        </mj-text>
      </mj-column>
    </mj-section>
    {% endfor %}

    {# ── Summary bar ── #}
    <mj-section background-color="#ffffff" padding="0 40px 32px">
      <mj-column>
        <mj-text padding="0">
          <div style="background-color:#1a1a2e;border-radius:6px;padding:16px 20px;">
            <table width="100%" cellpadding="0" cellspacing="0" style="border-collapse:collapse;">
              <tr>
                <td>
                  <span style="font-size:12px;color:#d1d5db;font-family:'DM Mono',monospace;">
                    &#128197; Next scheduled run:
                    <strong style="color:#ffffff;">{{ next_run }}</strong>
                  </span>
                </td>
                <td align="right">
                  <span style="font-size:12px;color:#fbbf24;font-family:'DM Mono',monospace;font-weight:600;">
                    &#9888; Action required: review flagged resources
                  </span>
                </td>
              </tr>
            </table>
          </div>
        </mj-text>
      </mj-column>
    </mj-section>

    {# ── Footer ── #}
    <mj-section background-color="#f9f9fb" padding="20px 40px"
                 border-top="1px solid #e5e7eb">
      <mj-column>
        <mj-text padding="0">
          <table width="100%" cellpadding="0" cellspacing="0" style="border-collapse:collapse;">
            <tr>
              <td valign="top">
                <p style="margin:0 0 6px;font-size:11px;color:#9ca3af;font-family:'DM Mono',monospace;">
                  &#9993; ERP Automation &middot; RCCP Module &middot; Build {{ build_version }}
                </p>
                <p style="margin:0;font-size:11px;color:#b0b0b8;line-height:1.6;">
                  This message was generated automatically. Do not reply directly to this email.<br>
                  Data source: SAP S/4HANA PP/DS &middot; Capacity Workbench &middot; Refresh cycle: daily at 06:00 UTC
                </p>
              </td>
              <td align="right" valign="top" style="padding-left:32px;white-space:nowrap;">
                <p style="margin:0 0 6px;">
                  <a href="{{ erp_url }}" style="font-size:11px;color:#6b6b80;text-decoration:none;font-family:Inter,Arial,sans-serif;">Open in ERP &#8599;</a>
                </p>
                <p style="margin:0 0 6px;">
                  <a href="{{ subscriptions_url }}" style="font-size:11px;color:#6b6b80;text-decoration:none;font-family:Inter,Arial,sans-serif;">Manage Subscriptions &#8599;</a>
                </p>
                <p style="margin:0;">
                  <a href="{{ support_url }}" style="font-size:11px;color:#6b6b80;text-decoration:none;font-family:Inter,Arial,sans-serif;">Support Portal &#8599;</a>
                </p>
              </td>
            </tr>
          </table>
        </mj-text>
      </mj-column>
    </mj-section>

    {# ── Bottom accent bar ── #}
    <mj-section padding="0">
      <mj-column>
        <mj-text padding="0" font-size="1px" line-height="3px">
          <div style="height:3px;background:linear-gradient(90deg,#1a1a2e 0%,#c0392b 100%);line-height:3px;font-size:1px;">&nbsp;</div>
        </mj-text>
      </mj-column>
    </mj-section>

  </mj-body>
</mjml>
