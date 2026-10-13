<script lang="ts">
  import { resolve } from '$app/paths';
  import { Button } from '$/components/ui/button';
  import { serializeState } from '$lib/util/serde';
  import { defaultState } from '$lib/constants';
  import type { State } from '$lib/types';

  interface Example {
    id: string;
    title: string;
    description: string;
    filename: string;
    code: string;
  }

  const examples: Example[] = [
    {
      code: `flowchart TD
    A[Solicitud de Compra] --> B{Monto > $5,000?}
    B -->|Sí| C[Aprobación Gerente]
    B -->|No| D[Aprobación Supervisor]
    C --> E{¿Aprobado?}
    D --> E
    E -->|Sí| F[Generar Orden de Compra]
    E -->|No| G[Rechazar Solicitud]
    F --> H[Enviar a Proveedor]
    G --> I[Notificar Solicitante]
    H --> J[Confirmar Recepción]
    J --> K[Fin del Proceso]
    I --> K`,
      description: 'Flujo de aprobación de solicitudes de compra según monto',
      filename: 'aprobacion-compras.mmd',
      id: 'aprobacion-compras',
      title: 'Aprobación de Compras'
    },
    {
      code: `sequenceDiagram
    participant RH as Recursos Humanos
    participant IT as Sistemas
    participant SUP as Supervisor
    participant EMP as Nuevo Empleado

    RH->>IT: Solicitar creación de cuentas
    IT->>IT: Crear usuario y email
    IT->>EMP: Enviar credenciales
    
    RH->>SUP: Notificar inicio de empleado
    SUP->>EMP: Dar bienvenida
    
    par Capacitaciones
        RH->>EMP: Inducción corporativa
    and
        SUP->>EMP: Entrenamiento del puesto
    end
    
    EMP->>RH: Confirmar finalización de onboarding
    RH->>RH: Actualizar registro de empleado`,
      description: 'Proceso de incorporación de nuevos empleados',
      filename: 'onboarding-empleado.mmd',
      id: 'onboarding-empleado',
      title: 'Onboarding de Empleado'
    },
    {
      code: `stateDiagram-v2
    [*] --> Nuevo: Usuario crea ticket
    
    Nuevo --> Asignado: Asignar a técnico
    Asignado --> EnProgreso: Técnico inicia trabajo
    
    EnProgreso --> Resuelto: Solución aplicada
    EnProgreso --> Escalado: Requiere especialista
    
    Escalado --> EnProgreso: Especialista asignado
    
    Resuelto --> Cerrado: Usuario confirma
    Resuelto --> EnProgreso: Usuario solicita revisión
    
    Cerrado --> [*]
    
    note right of Escalado
        Se escala si el tiempo
        de resolución > 24h
    end note`,
      description: 'Estados y transiciones de un ticket de soporte técnico',
      filename: 'ticket-soporte.mmd',
      id: 'ticket-soporte',
      title: 'Atención de Ticket de Soporte'
    },
    {
      code: `gantt
    title Proceso de Cierre Contable Mensual
    dateFormat YYYY-MM-DD
    
    section Recopilación
    Recopilar facturas           :a1, 2026-01-01, 3d
    Verificar documentación      :a2, after a1, 2d
    
    section Conciliación
    Conciliación bancaria        :b1, after a2, 2d
    Conciliación de cuentas      :b2, after b1, 2d
    
    section Ajustes
    Registrar ajustes            :c1, after b2, 1d
    Revisar asientos contables   :c2, after c1, 1d
    
    section Reportes
    Generar estados financieros  :d1, after c2, 2d
    Revisión final del Contador  :d2, after d1, 1d
    Aprobación de Gerencia       :milestone, after d2, 0d`,
      description: 'Cronograma de actividades del cierre contable',
      filename: 'cierre-contable.mmd',
      id: 'cierre-contable',
      title: 'Cierre Contable Mensual'
    }
  ];

  function getEditorUrl(code: string): string {
    const state: State = {
      ...defaultState,
      code
    };
    const serialized = serializeState(state);
    return resolve('/edit', {}) + '#' + serialized;
  }
</script>

<svelte:head>
  <title>Ejemplos Tecnasa - Mermaid Live Editor</title>
</svelte:head>

<div class="container mx-auto px-4 py-8">
  <div class="mb-8">
    <h1 class="mb-2 text-3xl font-bold">Ejemplos Tecnasa</h1>
    <p class="text-muted-foreground">
      Diagramas de procesos empresariales para el curso de Microsoft Copilot. Haz clic en cualquier
      ejemplo para abrirlo en el editor.
    </p>
  </div>

  <div class="grid gap-6 md:grid-cols-2">
    {#each examples as example (example.id)}
      <div class="rounded-lg border bg-card p-6 shadow-sm">
        <h2 class="mb-2 text-xl font-semibold">{example.title}</h2>
        <p class="mb-4 text-sm text-muted-foreground">{example.description}</p>
        <div class="flex gap-2">
          <Button href={getEditorUrl(example.code)} class="w-full">Abrir en Editor</Button>
        </div>
      </div>
    {/each}
  </div>

  <div class="mt-8 rounded-lg border bg-muted/50 p-6">
    <h3 class="mb-2 text-lg font-semibold">📁 Agregar más ejemplos</h3>
    <p class="text-sm text-muted-foreground">
      Los ejemplos se encuentran en la carpeta <code class="rounded bg-muted px-1 py-0.5"
        >examples/tecnasa/</code
      >. Para agregar un nuevo ejemplo:
    </p>
    <ol class="mt-3 list-decimal space-y-1 pl-5 text-sm text-muted-foreground">
      <li>
        Crea un archivo <code class="rounded bg-muted px-1 py-0.5">.mmd</code> con tu diagrama Mermaid
      </li>
      <li>Guárdalo en <code class="rounded bg-muted px-1 py-0.5">examples/tecnasa/</code></li>
      <li>Actualiza esta página para incluir el nuevo ejemplo en la galería</li>
    </ol>
  </div>
</div>
