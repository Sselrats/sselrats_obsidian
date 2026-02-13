
claude mcp add --transport http figma https://mcp.figma.com/mcp
{
	"google-sheets": {
          "command": "/Users/sselrats_m1pro/.local/bin/uvx",
          "args": ["mcp-google-sheets@latest"],
          "env": {
            "SERVICE_ACCOUNT_PATH": "/Users/sselrats_m1pro/codes/FinBox24/ld-engine/service-account.json",
            "DRIVE_FOLDER_ID": "1wqjybvg6exdEG7HomdFNatpRaK5m-k_L"
          }
       }
}

claude mcp add-json google-sheets '{
          "command": "/Users/sselrats_m1pro/.local/bin/uvx",
          "args": ["mcp-google-sheets@latest"],
          "env": {
            "SERVICE_ACCOUNT_PATH": "/Users/sselrats_m1pro/codes/FinBox24/ld-engine/packages/engine/service-account.json",
            "DRIVE_FOLDER_ID": "1wqjybvg6exdEG7HomdFNatpRaK5m-k_L"
          }
       }'