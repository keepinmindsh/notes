# Cursor에서 MCP 사용하기 ( 2025.08.03 )

## MCP 서버 활성화 경로 

- Cursor Settings > Tool & Integrations
  - Toggle을 이용한 설정 활성화 처리 



## MCP 서버 등록 방법 

- .cursor/mcp.json 파일에 MCP 서버 정보 등록

> 예시: Figma 연동

```json
{
  "mcpServers": {
    "Figma": {
      "url": "http://127.0.0.1:3845/sse"
    }
  }
}
```

## 커서 Rules에 대한 좋은 링크 

- [Cursor Rules Directories](https://cursor.directory/?q=flutter0)

## References 

> [Guide to The Dev Mode MCP Server](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server)

# Figma MCP 서버를 이용한 Flutter 개발하기 

## get_code
Use this to generate code for your Figma selection using the MCP server. The default output is React + Tailwind, but you can customize this through your prompts:

Change the framework
“Generate my Figma selection in Vue.”
“Generate my Figma selection in plain HTML + CSS.”
“Generate my Figma selection in iOS.”

### Use your components

“Generate my Figma selection using components from src/components/ui”
“Generate my Figma selection using components from src/ui and style with Tailwind”

## get_variable_defs
Returns variables and styles used in your selection—like colors, spacing, and typography.

### List all tokens used
“Get the variables used in my Figma selection.”
### Focus on a specific type
“What color and spacing variables are used in my Figma selection?”
### Get both names and values
“List the variable names and their values used in my Figma selection.”


## get_code_connect_map
Retrieves a mapping between Figma node IDs and their corresponding code components in your codebase. Specifically, it returns an object where each key is a Figma node ID, and the value contains:

- codeConnectSrc: The location of the component in your codebase (e.g., a file path or URL).
- codeConnectName: The name of the component in your codebase.

This mapping is used to connect Figma design elements directly to their React (or other framework) implementations,   
enabling seamless design-to-code workflows and ensuring that the correct components are used for each part of the design.   
If a Figma node is connected to a code component, this function helps you identify and use the exact component in your project.


## get_image
To use this tool, go to Preferences > Dev Mode MCP Server Settings > Enable tool get_image
This takes a screenshot of your selection to preserve layout fidelity. Keep this on unless you’re managing token limits.


## References 

> [Guide to The Dev Mode MCP Server](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server)