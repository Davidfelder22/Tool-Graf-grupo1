```mermaid
graph TD
    Alumno((Alumno))
    AgentePrincipal[Agente Principal<br/>Equipo Frontend]

    Alumno -->|1. Pide resolver| AgentePrincipal
    AgentePrincipal -->|2. Llama Tool| ToolIn[Tool Call:<br/>toolGraficadorFisica]

    subgraph Tu Infraestructura
        ToolIn -->|3. Valida JSON| SubAgente[Sub-Agente LLM<br/>Programador Python]
        SubAgente -->|4. Escribe Script| Sandbox[(Sandbox Ejecución)]
        Sandbox -->|5. Ejecuta| Outputs{Outputs}
        Outputs -->|stdout| JSONGeometrico[JSON coordenadas]
        Outputs -->|inlineData| ImagenBase64[Imagen PNG]
        
        JSONGeometrico --> Verificador{6. Verificador Geométrico}
        Verificador -->|7a. Falla regla| ErrorFeedback[Error al LLM]
        ErrorFeedback -.->|8. Reintento automático| SubAgente
        
        Verificador -->|7b. Pasa la prueba| ReturnBase[Return: Imagen]
    end

    ReturnBase -->|9. Devuelve Base64| AgentePrincipal
    AgentePrincipal -->|10. Muestra Gráfico| Alumno